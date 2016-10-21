---
title: hessian序列化的坑
comments: true
date: 2016-07-13 20:00:00
updated: 2016-07-13 20:00:00
tags:
  - java
  - dubbo
  - hessian
categories:
  - program
  - java
---

之前使用`dubbo`服务的过程中发生了一个关于`hessian`序列化引起的bug，有点意思，记录一下。

<!-- more -->

`hessian`序列化`Collections.emptyList()`时，会转换成`ArrayList()`，并且同一对象内会共用同一个`ArrayList`对象，例如：

**ProductDTO.java**
```java
/**
* 商品属性表DTO
*/
List<ProductAttributeDTO> productAttributeDTOs = Collections.emptyList();
/**
* 配件DTO
*/
List<PartsDTO> partsDTOs = Collections.emptyList();
/**
* 车型DTO
*/
List<CarTypeDTO> carTypeDTOs = Collections.emptyList();
```

以下代码：
```java
List<ProductAttributeNameDTO> productAttributeNameDTOs = queryProductAttributeNamesByCategoryId(topCategoryId);
List<ProductAttributeDTO> productAttributeDTOs = productDTO.getProductAttributeDTOs();// @1
Set<Integer> attrNameIds = Sets2.transform(productAttributeDTOs, ProductAttributeDTO::getAttrNameId);
productAttributeNameDTOs.stream()
       .filter(v -> !attrNameIds.contains(v.getAttrNameId()))
       .forEach(v -> {
           ProductAttributeDTO productAttributeDTO = ProductDTOTransformers.convert(v, new ProductAttributeDTO());
           productAttributeDTOs.add(productAttributeDTO);// @2
       });
```

以上代码的`productDTO`是从`dubbo`服务获取的，虽然`ProductDTO`的默认值是`Collections.emptyList()`（代码`@1`），但由于`Collections#EMPTY_LIST`并无实现序列化接口，`hessian`在序列化的过程中将其转换成`ArrayList`，并且同一对象内的`Collections.emptyList()`都会共用同一个`ArrayList`对象，造成执行`@2`操作的时候，`partsDTOs`和`carTypeDTOs`同样持有相同的`ArrayList`对象（java是伪泛型，运行期会抹除），所以当打开`debugger`或将`productDTO`序列化成`JSON`时会发现`productAttributeDTOs`, `partsDTOs`和`carTypeDTOs`的内容是一样的。
