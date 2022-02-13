# 结果返回List

- 绑定的接口方法返回`List<Object>`
- XML中`resultType`写上`<Object>`里的类型，不写List



# 结果返回Map

方式1:

- 绑定的接口方法返回`Map`
- XML中`resultType`指定为`map`
- 返回 `<属性名，属性值>`的键值对



方式2:

- 绑定的接口方法返回`Map`，方法前加上`MapKey("propertyName")`指定键值
- XML中`resultType`指定为`javaBean`
- 返回`<指定键值，javaBean>`的键值对