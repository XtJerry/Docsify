*今天无意查看到json格式，感觉有点怪，但是确实可以通过Gson解析*

**正常json格式[详解](http://www.json.org.cn/index.htm)**
{}对象
[]数组
:属性与值分隔符
,属性与属性分隔符
"属性标识或字符串
'属性标识

例子
```json
{"name":"test","id":123,"avatar":["../static/img/a.jepg","../static/img/a.jepg"]}

```

**其他类型Json格式**
``` json
{name=test,id=123,avatar=["../static/img/a.jepg","../static/img/a.jepg"]}

// 或者
{'name'=test,id=123,avatar=["../static/img/a.jepg","../static/img/a.jepg"]}

// test可以用"test",也可以直接写，但是如果值有特殊字符，需要用转义或者直接使用"test"

// 以上json可以使用gson正常解析,使用普通json检验工具会出错，其它未尝试；

```
**查看gson源码可知**
``` java
// GSON源码版本
public static final String VERSION = "2.8.5";
// gson
package com.google.gson;
public final class Gson {
	...
	// 将jsonString转为Object
	public <T> T fromJson(String json, Class<T> classOfT) throws JsonSyntaxException {
		Object object = fromJson(json, (Type) classOfT);
		return Primitives.wrap(classOfT).cast(object);
	}
	
	public <T> T fromJson(JsonReader reader, Type typeOfT) throws JsonIOException, JsonSyntaxException {
		...
		try {
			reader.peek();
			isEmpty = false;
			TypeToken<T> typeToken = (TypeToken<T>) TypeToken.get(typeOfT);
			TypeAdapter<T> typeAdapter = getAdapter(typeToken);//ReflectiveTypeAdapterFactory.Adapter
			T object = typeAdapter.read(reader);
		return object;
		} catch (EOFException e) {
			...
		}
		...
	}
	...
}


// gson部分源码
package com.google.gson.internal.bind;
public final class ReflectiveTypeAdapterFactory implements TypeAdapterFactory {
	 public static final class Adapter<T> extends TypeAdapter<T> {
	 	...
		@Override
		public T read(JsonReader in) throws IOException {
			...
			try {
				in.beginObject();
				while (in.hasNext()) {
					String name = in.nextName();//获取key
					BoundField field = boundFields.get(name);
					if (field == null || !field.deserialized) {
						in.skipValue();
					} else {
						field.read(in, instance);//设置值，具体解析大致是判断类型、再根据pos位置解析得到相应类型的值，最后放到属性中
					}
				}
			} catch (IllegalStateException e) {
				...
			}
			...
		}
	 }
	...
	
	...
	
	
}


// json解析类
package com.google.gson.stream;
public final class public class JsonReader implements Closeable {
	...
	public String nextName() throws IOException {
		int p = peeked;
		if (p == PEEKED_NONE) {
			p = doPeek();
		}
		String result;
		if (p == PEEKED_UNQUOTED_NAME) {
			result = nextUnquotedValue();//获取无引号的key,通过源码可以看出gson可以对无引用的key进行解析
		} else if (p == PEEKED_SINGLE_QUOTED_NAME) {
			result = nextQuotedValue('\'');//获取'单引号key
		} else if (p == PEEKED_DOUBLE_QUOTED_NAME) {
			result = nextQuotedValue('"');//获取"双引号的key
		} else {
			throw new IllegalStateException("Expected a name but was " + peek() + locationString());
		}
		peeked = PEEKED_NONE;
		pathNames[stackSize - 1] = result;
		return result;
	}
	...
}

``` 