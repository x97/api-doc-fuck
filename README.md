### api_doc_fuck

api_doc_fuck是一个让你爽到飞起的RESTful API 文档生成工具。 你可以用写代码的方式写文档，接下俩我们用例子说明一下。



我们依旧用图书出版社的例子， 这里涉及到两个角色。 作家、图书。于是我们定义他们数据类型。



```  python
from api_fuck import fields
class Author(fields.ObjectType):
    “”“ Author ”“”
    id = fields.AutoField('ID')
    name = fields.String("name", max_length=32, null=False, required=True, description="name of author")


class Book(fields.ObjectType):
    id = fields.AutoField('ID')
    name = fields.String(max_length=32, required=True, null=False)
    published_at = fields.Data()
    author = fields.RelatedField(Author, related_column_name="id", column_name='author_id')
```



我们定义好它们的数据类型后我们通过构造简单的RESTful后就能生成文档了。 



```python
from api_fuck import restful 

@restful.router(method={"create": "POST", "list": "GET", "retrieve": "GET"})
class BookAPI(restful.RESTAPI):
    class Meta:
        resourse = BooK
        create_fields = ['name', 'published', 'author_id']
        retrieve_fields = ['id', 'name', 'published_at', 'author']
        lookup_fields = 'id'
        
```



然后我们通过运行命令`api_fuck server ` 此时我们打开浏览器， 在`localhost:8000`中就可以访问刚才的文档了。 



当然你也可以通过命令`api_fuck buid --type=markdown` 生成markdown文档， 在build文件夹下可以看到生成的文档。 
