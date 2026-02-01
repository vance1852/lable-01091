# Elasticsearch 全文检索 C++ DEMO

基于 C++17 实现的 Elasticsearch 全文检索演示项目，展示如何使用 C++ 与 Elasticsearch 进行交互，实现索引管理、文档 CRUD 和全文检索功能。

## How to Run

### 方式一：Docker Compose（推荐）

```bash
# 1. 启动所有服务
docker-compose up --build -d

# 2. 等待 Elasticsearch 启动完成（约 30 秒）
docker-compose logs -f elasticsearch

# 3. 运行 C++ 演示程序
docker-compose run --rm cpp-demo

# 4. 停止服务
docker-compose down
```

### 方式二：本地编译运行

```bash
# 1. 启动 Elasticsearch
docker-compose up -d elasticsearch

# 2. 编译 C++ 项目
cd backend
mkdir build && cd build
cmake ..
make

# 3. 运行程序
./es_demo
```

## Services

| 服务          | 端口 | 说明                       |
| ------------- | ---- | -------------------------- |
| Elasticsearch | 9200 | 搜索引擎服务               |
| Kibana        | 5601 | ES 可视化管理（可选）      |
| cpp-demo      | -    | C++ 演示程序（一次性运行） |

### 访问地址

- Elasticsearch: http://localhost:9200
- Kibana: http://localhost:5601

## 测试账号

本项目为纯技术演示，无需登录认证。

Elasticsearch 默认配置：

- 用户名：elastic（如启用安全模式）
- 密码：changeme（如启用安全模式）

## 题目内容

> 使用 C++ 语言，搭建一个 ES 的全文检索的 DEMO

---

## 功能特性

### 索引管理

- ✅ 创建索引（支持自定义 mapping）
- ✅ 删除索引
- ✅ 查看索引信息

### 文档操作

- ✅ 添加文档
- ✅ 批量添加文档
- ✅ 获取文档
- ✅ 更新文档
- ✅ 删除文档

### 全文检索

- ✅ Match 查询（分词匹配）
- ✅ Term 查询（精确匹配）
- ✅ Bool 组合查询
- ✅ 高亮显示
- ✅ 分页查询
- ✅ 中文分词支持

## 技术栈

- **语言**: C++17
- **HTTP 客户端**: libcurl
- **JSON 处理**: nlohmann/json
- **搜索引擎**: Elasticsearch 8.x
- **构建工具**: CMake 3.16+
- **容器化**: Docker & Docker Compose

## 项目结构

```
.
├── backend/                 # C++ 后端代码
│   ├── CMakeLists.txt      # CMake 构建配置
│   ├── Dockerfile          # Docker 镜像构建
│   ├── include/            # 头文件
│   │   ├── es_client.hpp   # ES 客户端类
│   │   ├── http_client.hpp # HTTP 客户端类
│   │   └── json.hpp        # nlohmann/json 库
│   ├── src/                # 源代码
│   │   ├── main.cpp        # 主程序入口
│   │   ├── es_client.cpp   # ES 客户端实现
│   │   └── http_client.cpp # HTTP 客户端实现
│   └── data/               # 示例数据
│       └── sample_data.json
├── docs/                   # 文档
│   └── project_design.md   # 项目设计文档
├── docker-compose.yml      # Docker Compose 配置
├── .gitignore             # Git 忽略文件
└── README.md              # 项目说明
```

## 使用示例

程序运行后会自动执行以下演示：

1. **创建索引** - 创建名为 `articles` 的索引，配置中文分词
2. **批量导入** - 导入示例文章数据
3. **全文检索** - 演示各种搜索方式
4. **高亮显示** - 展示搜索结果高亮
5. **清理资源** - 删除测试索引

### 输出示例

```
========================================
  Elasticsearch C++ 全文检索 DEMO
========================================

[1] 创建索引 'articles'...
✓ 索引创建成功

[2] 批量导入文档...
✓ 成功导入 5 篇文章

[3] 全文检索演示...

--- Match 查询: "人工智能" ---
命中 2 条结果:
  [1] 人工智能的发展历程 (score: 8.234)
  [2] 机器学习入门指南 (score: 5.123)

--- 高亮搜索: "深度学习" ---
  标题: 深度学习实战
  高亮: ...<em>深度学习</em>是机器学习的一个分支...

[4] 清理资源...
✓ 索引删除成功

========================================
  演示完成！
========================================
```

## 扩展开发

### 添加新的搜索功能

```cpp
// 在 es_client.hpp 中添加新方法
SearchResult fuzzySearch(const std::string& index,
                         const std::string& field,
                         const std::string& value,
                         int fuzziness = 2);
```

### 自定义索引 Mapping

```cpp
json mapping = {
    {"properties", {
        {"title", {{"type", "text"}, {"analyzer", "ik_max_word"}}},
        {"content", {{"type", "text"}, {"analyzer", "ik_smart"}}},
        {"tags", {{"type", "keyword"}}},
        {"created_at", {{"type", "date"}}}
    }}
};
client.createIndex("my_index", mapping);
```

## License

MIT License
