swagger-bootstrap-ui
=========================
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.xiaoymin/swagger-bootstrap-ui/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.github.xiaoymin/swagger-bootstrap-ui)



## 简介

基于swagger-bootstrap-ui做了一些优化拓展，原地址是在 [swagger-bootstrap-ui](https://github.com/xiaoymin/Swagger-Bootstrap-UI) 访问,一些特性功能可以在原地址上进行参考.本项目没有打包到mavne私服中,**需要自己本地编译。**



## 功能展示

注解使用

![注解配置图](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MzcwOTg1LWMyZDBmMDUyYTIxMTAzZGMucG5n)

1. 入参

![image.png](https://upload-images.jianshu.io/upload_images/6370985-1af4bf76367a250e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 1.1 多层级的入参

![多层级的入参](https://upload-images.jianshu.io/upload_images/6370985-f7f8f7c333b80dd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 出参

![image.png](https://upload-images.jianshu.io/upload_images/6370985-f8fd1c4da6ec153b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/6370985-c680b00df205f955.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	多层级出参 : 

![多层级出参](https://upload-images.jianshu.io/upload_images/6370985-7a34c137e70c315f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



1. 调试

   ![image.png](https://upload-images.jianshu.io/upload_images/6370985-0018c09ab2ac2180.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   ![响应内容](https://upload-images.jianshu.io/upload_images/6370985-4ae3fd76f3316f0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 特殊特性

- 支持Model中的属性特殊化隐藏调用

> 例如 : 增加的时候 id 是不用填写的, 但是修改的时候是必填的, 他们共用一个实体,但只是某些属性在某些场景特殊化 , 这里采取的一种方案就是,将对应的接口URL作为标识,匹配的将触发对应的属性,这里目前只**支持隐藏和必填属性**

```java
 @ApiModelProperty(name = "id", position = 0, value = "项目编号[添加操作可不传递,修改必传]",
            extensions = {
              		// 存在与hiddenList中的路径,在展示和这个路径相关的接口时,会触发这里面的hidden方法
                    @Extension(name = "hiddenList", properties = {
                            @ExtensionProperty(name = "/project/insert", value = "true")
                    }),
                    @Extension(name = "requireList", properties = {
                            @ExtensionProperty(name = "/project/update", value = "true"),
                    })
            })
    @Id
```

这里的swagger的注解版本必须1.5.19以上

```xml
<dependency>
   <groupId>io.swagger</groupId>
   <artifactId>swagger-annotations</artifactId>
   <scope>compile</scope>
   <version>1.5.19</version>
</dependency>
```





## 使用

本地编译过后,根据自己拓展的版本号自己去更改

```
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.7.3.1</version>
</dependency>
```

swagger 相关的jar包

```xml
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger2</artifactId>
</dependency>

<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger-ui</artifactId>
</dependency>

<dependency>
  <groupId>io.swagger</groupId>
  <artifactId>swagger-annotations</artifactId>
  <scope>compile</scope>
  <version>1.5.19</version>
</dependency>
```



model层:

```java
@ApiModel(description = "项目管理实体")
public class ProjectEntity {
    // id
    // 表字段 : tfb_project.id
    @ApiModelProperty(name = "id", position = 0, value = "项目编号[添加操作可不传递,修改必传]",
            extensions = {
                    @Extension(name = "hiddenList", properties = {
                            @ExtensionProperty(name = "/project/insert", value = "true")
                    }),
                    @Extension(name = "requireList", properties = {
                            @ExtensionProperty(name = "/project/update", value = "true"),
                    })
            })
    @Id
    private Integer id;

    // 项目名称
    // 表字段 : tfb_project.name
    @ApiModelProperty(name = "name", value = "项目名称", required = true, position = 1)
    private String name;

    // 省
    // 表字段 : tfb_project.province
    @ApiModelProperty(name = "province", value = "省份", required = true, position = 2, example = "湖南省")
    private String province;

    // 市
    // 表字段 : tfb_project.city
    @ApiModelProperty(name = "city", value = "城市", required = true, position = 3, example = "长沙市")
    private String city;

    // 区
    // 表字段 : tfb_project.district
    @ApiModelProperty(name = "district", value = "地区", required = true, position = 4, example = "雨花区")
    private String district;

    // 项目地址
    // 表字段 : tfb_project.address
    @ApiModelProperty(name = "address", value = "地址", position = 5, required = true)
    private String address;

    // 开发商
    // 表字段 : tfb_project.owner
    @ApiModelProperty(name = "owner", value = "开发商", position = 6, required = true, allowableValues = "人名")
    private String owner;

    // 合作开始时间
    // 表字段 : tfb_project.startdate
    @ApiModelProperty(name = "startdate", value = "合作开始时间", position = 7, required = true, allowableValues = "yyyy-MM-dd HH:mm:ss")
    private Date startdate;

    // 合作结束时间
    // 表字段 : tfb_project.enddate
    @ApiModelProperty(name = "enddate", value = "合作结束时间", position = 8, required = true, allowableValues = "yyyy-MM-dd HH:mm:ss")
    private Date enddate;

    // 项目电话
    // 表字段 : tfb_project.tel
    @ApiModelProperty(name = "tel", value = "合作方手机号码", position = 9, required = true)
    private String tel;

    // 状态  1 有效  -1 无效
    // 表字段 : tfb_project.status
    @ApiModelProperty(name = "status", value = "是否有效", position = 10, required = false, allowableValues = "1,-1")
    private Integer status;

    // 创建时间
    // 表字段 : tfb_project.created
    @ApiModelProperty(name = "created", value = "创建时间[系统操作]", position = 13, required = false)
    private Date created;

    // 创建者
    // 表字段 : tfb_project.creator
    @ApiModelProperty(name = "creator", value = "创建人", position = 14, required = false)
    private String creator;

    // 修改时间
    // 表字段 : tfb_project.updated    @ApiModelProperty(name = "creator", value = "创建人", position = 14 ,required = false)
    @ApiModelProperty(name = "updated", value = "修改时间", position = 15, required = false)
    private Date updated;

    // 修改者
    // 表字段 : tfb_project.updator
    @ApiModelProperty(name = "updator", value = "修改人", position = 15, required = false)
    private String updator;

    // 小程序二维码
    // 表字段 : tfb_project.miniapp_qrcode
    @ApiModelProperty(name = "miniappQrcode", value = "小程序二维码", position = 11, required = false)
    @Column(name = "miniapp_qrcode")
    private String miniappQrcode;

    // app下载二维码
    // 表字段 : tfb_project.app_qrcode
    @ApiModelProperty(name = "appQrcode", value = "app下载二维码", position = 12, required = false)
    @Column(name = "app_qrcode")
    private String appQrcode;

    @ApiModelProperty(name = "addressExt", value = "地址 - 拓展", position = 5, required = false)
    @Ignore
    private String addressExt;
   // .... getset
}
```



controller:

```java
	/**
     * 添加项目管理模块
     *
     * @param request
     * @return
     * @throws Exception
     */
    @ApiOperation(value = "添加项目", notes = "添加项目管理", nickname = "liukx")
    @RequestMapping(value = "/insert", method = RequestMethod.POST, produces = "application/json;charset=UTF-8")
    @ResponseBody
    public ResponseCommonModel insertProject(@RequestBody ProjectEntity request) throws Exception {
        int result = projectService.insert(request);
        ResponseCommonModel response = new ResponseCommonModel();
        if (result < 0) {
            response.setSuccess(false);
        }
        return response;
    }
```



**另外基于Swagger的拓展,这里只是为了让swagger支持`extensions`拓展属性**

```java
import com.fasterxml.jackson.databind.introspect.BeanPropertyDefinition;
import com.google.common.base.Function;
import com.google.common.base.Optional;
import io.swagger.annotations.ApiModelProperty;
import io.swagger.annotations.Extension;
import io.swagger.annotations.ExtensionProperty;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;
import springfox.documentation.schema.Annotations;
import springfox.documentation.service.ObjectVendorExtension;
import springfox.documentation.service.StringVendorExtension;
import springfox.documentation.service.VendorExtension;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spi.schema.ModelPropertyBuilderPlugin;
import springfox.documentation.spi.schema.contexts.ModelPropertyContext;
import springfox.documentation.spring.web.DescriptionResolver;
import springfox.documentation.swagger.common.SwaggerPluginSupport;

import java.util.ArrayList;
import java.util.List;

@Component
@Order(2)
public class MyApiModelPropertyPropertyBuilder implements ModelPropertyBuilderPlugin {
    private final DescriptionResolver descriptions;

    @Autowired
    public MyApiModelPropertyPropertyBuilder(DescriptionResolver descriptions) {
        this.descriptions = descriptions;
    }

    public void apply(ModelPropertyContext context) {
        Optional<ApiModelProperty> annotation = Optional.absent();

        if (context.getBeanPropertyDefinition().isPresent()) {
            annotation = annotation.or(Annotations.findPropertyAnnotation((BeanPropertyDefinition) context.getBeanPropertyDefinition().get(), ApiModelProperty.class));
        }

        if (annotation.isPresent()) {
            context.getBuilder().extensions(annotation.transform(toExtension()).orNull());
        }

    }

    public boolean supports(DocumentationType delimiter) {
        return SwaggerPluginSupport.pluginDoesApply(delimiter);
    }


    static Function<ApiModelProperty, List<VendorExtension>> toExtension() {
        return new Function<ApiModelProperty, List<VendorExtension>>() {
            public List<VendorExtension> apply(ApiModelProperty annotation) {
                Extension[] extensions = annotation.extensions();
                List<VendorExtension> list = new ArrayList<>();
                if (extensions != null && extensions.length > 0) {
                    for (int i = 0; i < extensions.length; i++) {

                        Extension extension = extensions[i];
                        String name = extension.name();
                        ObjectVendorExtension objectVendorExtension = new ObjectVendorExtension(name);
                        if (!name.equals("")) {
                            for (int j = 0; j < extension.properties().length; j++) {
                                ExtensionProperty extensionProperty = extension.properties()[j];
                                StringVendorExtension stringVendorExtension = new StringVendorExtension(extensionProperty.name(), extensionProperty.value());
                                objectVendorExtension.addProperty(stringVendorExtension);
                            }
                            list.add(objectVendorExtension);
                        }
                    }
                }
                return list;
            }
        };
    }

}

```



上述流程基本上就可以达到使用的目的了



再次声明 原作者 : [xiaoymin](https://github.com/xiaoymin/Swagger-Bootstrap-UI)

该项目只是基于原基础上进行了一些改造,非常感谢原作者的开源项目节省了大量的研发成本。
