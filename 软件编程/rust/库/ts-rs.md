
- `../`  代表上一层目录

```rust
#[derive(Debug, Clone, Serialize)]
#[derive(TS)]
#[ts(export,export_to = "../../src/types/")]
pub struct RowData {
    pub id: usize,
    pub bom_level: String,
    pub material_code: String,
    pub material_name: String,
    pub material_model:String,
    pub material_quantity:i32,
    pub sub_rows: Vec<RowData>,
}
```