% View- and Dependency Injection
% Patrick Sturm
% 23.09.2015

## Frameworks

* We will be looking at two different frameworks today
	* Butter Knife
	* AndroidAnnotations
* Both frameworks provide elegant ways of resource / listener injection
* We will not be looking at dagger today, as a task for you at home: look at dagger (2)

## Recap Annotations 1
```java
// accessiblity of annotation
@Retention(RetentionPolicy.RUNTIME)
// this annotation is only applicable to...
@Target(ElementType.FIELD)
// the actual annotation definition
public interface @MyAnnotation {
	// annotation parameter definition
	int someParameter() default 0;
}
```
## Recap Annotations 2
* Retention (can be left out):
	* SOURCE: annotation only available in source code (e.g.: compile time checks)
	* CLASS: default, available in class file and source code (e.g.: compile time checks in external lib)
	* RUNTIME: available at runtime, in class file and source code (everything that requires runtime checks)
* Target
	* Defines what kind of construct can be annotated using this annotation (can be left out)
	* Can be a number of things including: FIELD, METHOD and TYPE

## Recap Annotations 3
```java
@MyAnnotation(1)
private Object testField;
```

## Recap Annotations 3
```java
MyAnnotation a = (MyAnnotation) myObject
	.getClass()
	.getDeclaredField("testField")
	.getAnnotation(MyAnnotation.class);		   
// getting our value!
a.testField();
```

## Butter Knife 1

* Butter Knife could be described as a micro framework
* It does two things (and does them well):
	* Resource Injection
	* Listener Bindings
* If you want something more sophisticated and less focused, AndroidAnnotations might be more to your liking

## Butter Knife 2

* Butter Knife supports the following Annotations:
	* @Bind: allows view bindings
	* @Bind[Drawable,Bool,Color,Dimen,Int,String]: binds resource type
	* @OnClick: mark method as click listener for specified button

## Butter Knife 3

* Butter Knife works by utilizing "composition", rather than inheritance
* In order to make Butter Knife aware of something, we need to manually bind Butter Knife to the component in question, likewise, we need to unbind Butter Knife when we are done
* Use: ButterKnife.bind(targetClass), ButterKnife.unbind(targetClass), Butter Knife also supports providing Activities, Dialogs and Views as secondary parameters, which is important if we want to bind views of Fragments!
* Please note that fields, which we want to inject using Butter Knife must not be `private`

## Butter Knife 4
[CODE EXAMPLE!](https://github.com/SphericalElephant/android-example-butterknife)

## AndroidAnnotations 1

* AndroidAnnotations support a number of different features, including view injection and listener binding!
* Not as focused as Butter Knife:
	* Supports "real" dependency injection and resource injection
	* Supports event binding
	* Thread binding
	* REST API binding
	* Android Preference API helpers / typesafty

## AndroidAnnotations 2
* Since AndroidAnnotations is a very large framework, we will not cover everything here
* We will only cover view injection and event binding
* Unlike other Android frameworks, AndroidAnnotations is well documented
* See these resources for more information on the topic [here](https://github.com/excilys/androidannotations/wiki/AvailableAnnotations)

## AndroidAnnotations 3
* Unlike Butter Knife, which needs to be bound manually, AndroidAnnotations will generate code from annotated classes
* That also means that we need to be careful when calling our components in Java code or XML.
	* Our Activity / Fragment / Service / ContentProvider / etc is not the one that will be used during runtime, we need to reference the actual component by denoting (postfixing) it with an "\_", MainActivity.class becomes MainActivity\_.class, even in AndroidManifest.xml declaration!
* Generated files can be found in build/generated/source/apt/debug

## AndroidAnnotations 4
* AndroidAnnotations features a set of annotations to make the framework recognize these classes as annotated (Enhanced Components)
* Please note that fields, which we want to inject using AndroidAnnotations must not be `private`

## AndroidAnnotations 5
* We will cover all annotations that allow us to mimic the behavior of Butter Knife:
	* @ViewById: corresponds to @Bind
	* @Click: corresponds to @OnClick
	* @[]Res: Corresponds to @Bind[]

## AndroidAnnotations 6
[CODE EXAMPLE!](https://github.com/SphericalElephant/android-example-androidannotations)