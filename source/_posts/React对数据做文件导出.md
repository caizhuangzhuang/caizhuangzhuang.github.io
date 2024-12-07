---
title: React导出excel表格
date: 2023-08-09 22:37:32
tags:
categories: 前端知识
---

```javascript
import * as XLSX from "xlsx";
function Tableexcel() {
  const data = [
    {
      name: "Jone",
      age: 25,
      city: "北京",
    },
    {
      name: "张三",
      age: 26,
      city: "上海",
    },
    {
      name: "李四",
      age: 26,
      city: "杭州",
    },
  ];

  const exportToExcel = (data) => {
    // 创建一个新的工作簿
    const workbook = XLSX.utils.book_new();

    // 将数据转换为一个工作表
    const worksheet = XLSX.utils.json_to_sheet(data);

    // 将工作表添加到工作簿中
    XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");

    // 导出工作簿为Excel文件
    XLSX.writeFile(workbook, "example.xlsx");
  };
  return (
    <>
      <button onClick={() => exportToExcel(data)}>导出excel</button>
    </>
  );
}
export default Tableexcel;
```
