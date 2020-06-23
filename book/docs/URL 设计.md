# URL 设计

* [动词 + 宾语](#动词--宾语)
* [动词的覆盖](#动词的覆盖)
* [宾语必须是名词](#宾语必须是名词)
* [复数 URL](#复数-url)
* [避免多级 URL](#避免多级-url)

## 动词 + 宾语

客户端发送的数据和操作指令都是以动词 + 宾语的结构。比如 `GET /articles` 这条命令中， `GET` 是动词， `/articles` 是宾语。动词通常有以下几种 HTTP 请求方法，对应程序的 CURD 操作。

```
# GET：读取（Read）
# POST：新建（Create）
# PUT：更新（Update）
# PATCH：更新（Update），通常是部分更新
# DELETE：删除（Delete）
```

## 动词的覆盖

有些客户端只能使用 GET 和 POST 这两种方法，此时服务器必须接受 POST 模拟其他三个方法（ PUT 、 PATCH 、 DELETE ）。客户端发出的 HTTP 请求，要加上 `X-HTTP-Method-Override` 属性，告诉服务器应该使用哪一个动词来覆盖 POST 方法。

```
POST /articles/4 HTTP/1.1
X-HTTP-Method-Override: PUT
```

上面代码中， `X-HTTP-Method-Override` 指定本次请求的方法是 PUT ，而不是 POST 。

## 宾语必须是名词

宾语就是 API 的 URL ，是 HTTP 动词作用的对象，它应该是名词，不能是动词。比如， `/articles` 这个 URL 是正确的，而下面的 URL 不是名词，都是错误的。

```
# /getAllCars
# /createNewCar
# /deleteAllRedCars
```

## 复数 URL

既然 URL 是名词，那么名词应该使用复数形式还是单数形式？这没有统一的规定，但是常见的操作是读取一个集合，比如 `GET /articles` 用来读取所有的文章，这里明显应该是复数形式。因此，为了统一起见，建议都使用复数 URL 。

## 避免多级 URL

常见的情况是，资源需要多级分类，因此很容易写出多级的 URL ，比如获取某个作者的某一类文章。

```
# GET /authors/12/categories/2
```

这种 URL 不利于扩展，语义也不明确，往往要想一会，才能明白含义。而更好的做法是，除了第一级，其他级别都用查询字符串表达。

```
# GET /authors/12?categories=2
```

下面是另一个例子，用来查询已发布的文章。

```
# GET /articles?published=true
```

