# wp-conf-base

![CI](https://github.com/wp-labs/wp-conf-base/workflows/CI/badge.svg)
[![codecov](https://codecov.io/gh/wp-labs/wp-conf-base/graph/badge.svg?token=6SVCXBHB6B)](https://codecov.io/gh/wp-labs/wp-conf-base)


`wp-conf-base` 是 WarpParse 配置体系中的底层 crate，封装了通用的配置建模、解析与校验工具，供各个 connector 或上层配置仓库复用。核心能力包括：

- 统一的 connector kind 适配接口 `ConnectorKindAdapter`，用于描述连接器的默认参数、URL 解析逻辑与默认格式。
- 配置实体通用操作 `ConfStdOperation`/`Validate`，用于加载、初始化与校验配置文件。
- `Protocol` 等基础枚举/类型，确保配置值在解析阶段就能被校验。
- Tags 解析与校验工具，提供 `ConfParser<[String]>` 实现并约束 key/value 的长度与字符集。
- 对布尔开关的兼容反序列化函数 `de_bool_onoff`，接受 `on/off`、`true/false`、`1/0` 以及 `yes/no` 等多种表达。

## 快速开始

在 `Cargo.toml` 中加入依赖：

```toml
[dependencies]
wp-conf-base = "0.7"
```

然后在代码中复用通用构件，例如自定义连接器 Kind：

```rust
use serde_json::json;
use wp_conf_base::connector::ConnectorKindAdapter;
use wp_connector_api::ParamMap;

struct MysqlAdapter;

impl ConnectorKindAdapter for MysqlAdapter {
    fn kind(&self) -> &'static str { "mysql" }

    fn defaults(&self) -> ParamMap {
        let mut params = ParamMap::new();
        params.insert("host".into(), json!("127.0.0.1"));
        params.insert("port".into(), json!(3306));
        params
    }
}
```

或在配置结构中沿用 `ConfStdOperation` 与 `de_bool_onoff`：

```rust
use orion_variate::EnvDict;
use serde::Deserialize;
use wp_conf_base::structure::{ConfStdOperation, Validate};
use wp_conf_base::de_bool_onoff;

#[derive(Deserialize, Default)]
struct OutputConf {
    path: String,
    #[serde(default, deserialize_with = "de_bool_onoff")]
    enabled: bool,
}

impl ConfStdOperation for OutputConf {
    fn load(path: &str, dict: &EnvDict) -> orion_conf::error::OrionConfResult<Self> {
        let raw = std::fs::read_to_string(path)?;
        let hydrated = raw.env_eval(dict);
        let conf: OutputConf = toml::from_str(&hydrated)?;
        conf.validate()?;
        Ok(conf)
    }

    fn init(_: &str) -> orion_conf::error::OrionConfResult<Self> {
        Ok(OutputConf::default())
    }

    fn safe_clean(_: &str) -> orion_conf::error::OrionConfResult<()> {
        Ok(())
    }
}

impl Validate for OutputConf {}
```

Tags 构造/校验同样可以直接依赖 `ConfParser` 的实现：

```rust
use wp_conf_base::ConfParser;
use wp_connector_api::Tags;

let tags = Tags::from_parse(&vec!["env:prod".to_string(), "debug".to_string()]);
Tags::validate(&vec!["env:prod".to_string()])?;
```

## 本地开发

```bash
# 运行单元测试
cargo test
```

测试涵盖协议解析、布尔兼容反序列化、tag 规则以及 `ConfStdOperation::try_load` 的基本行为，可作为扩展新功能时的回归保障。

## 许可

本仓库遵循 Elastic License 2.0 (`Elv2`)。
