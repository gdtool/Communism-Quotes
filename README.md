# 中文马克思主义文库检索系统

> 马克思、恩格斯、列宁、斯大林的理论，是“放之四海而皆准”的理论。
> 不应当把他们的理论当作教条看待，而应当看作行动的指南。
> 不应当只是学习马克思列宁主义的词句，而应当把它当成革命的科学来学习。
> 不但应当了解马克思、恩格斯、列宁、斯大林他们研究广泛的真实生活和革命经验所得出的关于一般规律的结论，而且应当学习他们观察问题和解决问题的立场和方法。
> —— 毛泽东 <中国共产党在民族战争中的地位> 一九三八年十月十四日

![Github Social Coding](docs/git_social.jpg)

本项目是中文马克思主义著作的全文检索系统。
用户可以输入关键字以及其他信息，以获取匹配程度最高的马克思主义著作原句。

本仓库包含以下内容：

## 内容列表

- [背景](#背景)
- [安装](#安装)
- [使用](#使用)
- [维护情况](#维护情况)
- [如何贡献](#如何贡献)
- [使用许可](#使用许可)


## 背景

当我们需要查阅关于某些话题的经典论述时，经常因忘记了具体的措辞而无所适从。
因为我们无法记提供这些名词的精确表述，所以我们就无法在某些事情发生的时候查一查历史上是否有先辈曾经做出过相关的论述。
我们需要一套可以提供文本关联和推荐排序的搜索系统。

## 安装

### 依赖环境

- `Python 3.7`（建议直接使用 `Anaconda` ）
- `ElasticSearch 7.5.0` （核心依赖组件）
- `MySQL 8.0` （目前不是强制需要）

### 安装方式

1. 下载项目源码到本地，并进入根目录
    ```
    git clone https://github.com/nevertiree/Communism-Quotes.git 
    cd Communism-Quotes/
    ```

2. 安装相关 `Python` 依赖包

   使用pip 
   ```
   pip install -r requirements.txt
   ```
   或者使用pipenv
   ```
   pipenv install --skip-lock
   ``` 
    
3. 下载 [`ElasticSearch`](https://www.elastic.co/cn/downloads/elasticsearch)

    在官方网站上下载 `ElasticSearch` 并解压。
    
4. 下载中文分词插件 [`IK Analysis for Elasticsearch`](https://github.com/medcl/elasticsearch-analysis-ik)

    进入 `ElasticSearch` 的根目录，通过 `bin/elasticsearch-plugin`命令来安装中文分词插件。
    需要注意分词插件的版本号需要与 `ElasticSearch` 的版本号对应，本项目目前在 `7.5.0` 版本下测试。
    
    ```
    ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.5.0/elasticsearch-analysis-ik-7.5.0.zip
    ```
   
    如果无法通过 `bin/elasticsearch-plugin` 完成安装，那么可以直接下载分词插件的压缩包。
    把分词插件解压到 `ElasticSearch` 根目录下的 `plugins` 文件中，把文件夹命名为 `analysis-ik`。
    
    ```
    /path_to_elasticsearch-7.5.0/plugins/analysis-ik>
        -a----         commons-codec-1.9.jar
        -a----         commons-logging-1.2.jar
        -a----         elasticsearch-analysis-ik-7.5.0.jar
        -a----         httpclient-4.5.2.jar
        -a----         httpcore-4.4.4.jar
        -a----         plugin-descriptor.properties
        -a----         plugin-security.policy
    ```

5. 下载文库资料

    Git仓库中没有直接存储文库资料，可以在 `Release` 界面下载。
    后续会实现稳定的下载机制。
    文库资料统一放在 `material` 文件夹中，详情可查看该文件夹中的说明。
   
## 使用

### 初次使用

1. 运行 `ElasticSearch` 

   Linux系统中运行`bin/elasticsearch`，Windows系统中运行` bin\elasticsearch.bat `。
   打开 http://localhost:9200/ 检查是否成功运行。

2. 运行 `setup.py` 文件进行初始化

    ```
    python setup.py
    ```
   
3. 运行 `main.py` 文件进行全文检索
    
   ```
   python main.py
   ```

### 更多介绍

  1.文献解析

所有的文献都存储在 `material` 文件的各个子文件夹中。
文件夹`util`中存放用于解析、清洗和导入文本数据的若干个工具类。

  2.文献存储

本项目的设计思路是“存储-查询”解耦。`storage`目录存放了两个文件存储函数，分别用于ElasticSearch和MySQL。

用户可以直接把HTML数据解析导入到ElasticSearch，然后用ElasticSearch进行检索，即存储检索一体化。
用户也可以把HTML数据解析导入到MySQL中，然后用迁移工具把文献从MySQL导入到ElasticSearch中。

这种设计思路的初衷是维护数据的稳定性——相比于ElasticSearch而言，各个MySQL版本之间更加问题。
目前这个设计思路还没有彻底实现，用户可以通过一体化的方式来存储与查询。

  3.文献查询

目录 `query` 中存放了进行全文搜索的函数。

## 维护情况

### 文库插件支持度

|  作者          | 文件夹名      | 完成度 |
|  :---:         | :---:        | :---: |
| 列宁（列宁全集）| lenin-cworks | 完成 |
| 毛泽东         | maozedong     | 完成 |
| 马克思         | marx          | |
| 恩格斯         | engels        | |
| 马恩选集       | marx-engels   | |
| ...            | ...           | |

### 维护者

[@nevertiree](https://github.com/nevertiree)

## 如何贡献

非常欢迎你的加入! 
[提一个Issue](https://github.com/nevertiree/Communism-Quotes/issues/new)
或者提交一个 `Pull Request`.

本项目遵循 [Contributor Covenant](http://contributor-covenant.org/version/1/3/0/) 行为规范。

## 使用许可

[MIT](LICENSE) @ Nevertiree
