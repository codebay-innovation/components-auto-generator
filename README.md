# Issue repository
This is the main repository for report issues related with the vscode extension "AEM Components Auto Generator"

# AEM Components Auto Generator [![Codebay Innovation](https://www.codebay-innovation.com/components-auto-generator/codebay_logo.png)](https://www.codebay-innovation.com/)
It transforms an HTML with a given information to a fully functional component, it also adds the model of the component.

## CodeBay Framework to generate AEM components
[![Powered By Codebay Innovation](https://img.shields.io/badge/Powered%20By-Codebay%20Innovation-37c755?labelColor=27d1e0)](https://www.codebay-innovation.com/)
[![Build Status](https://img.shields.io/badge/build-v1.0.12-lightgrey)]()

## How to use it:

1. Press ctrl+shift+p (In windows) or Command+shift+p (In IOs) and search for the plugin.
2. Once pressed, the plugin should activated and started.

## Working with the plugin

How to work with the plugin:

### Workflow
The first thing to mention is how the plugin works:
1. The plugin asks an input that must follow some specific semantic rules.
2. Once the input has been given, the plugin creates the components
3. The order of the component creation is from the top of the given HTML to the bottom.
4. In each component:
    1. It updates the general component info, HTML and dialog in that specific order
    2. It created the model and the package-info of the created models if the file does not exists.

Note: If an absolute path is given, it will create the models in *theGivenFolder/model*

### Semantic rules
The way the plugin works is by inserting an HTML structure with specified properties where it will get all the information.

You must follow a structure and semantic rules to correctly create a component.
1. All components begin with an element in wich you need to specify the component path and package of the model.
2. After the path you can insert as many elements as components you want to create, in this elements the name and the group of each component needs to be specified.
3. Insert inside these new elements as many properties as necessary.

Here is a list of the properties and its uses:

* Structure: Defines all the structure of the component.
    * aem-component-path: Specifies the path where the component will be created.
        * NOTE: You can specify relative and absolute path, to specify a relative path, you must pass it beggining in the ui.apps folder of your project and in the absolute path, it must start with "/" or "\".
        * Use case:
          ```html
            <div aem-component-path="ui.apps\src\main\content\jcr_root\apps\my-super-project\components" ...>
              ...
            </div> 
          ```
    * aem-models-package: Package that will be used in the model of the component.
        * NOTE: The plugin will create the package-info.java files if they not exists and create a structure folder if the name includes more folders. Read note of aem-component-name for more information.
        * Use case: 
          ```html
          <div aem-models-package="com.myproject.core.models" ...>
            ...
          </div> 
          ```
    * aem-component-name: The name of the component.
        * NOTE: If some folders are specified in the name, it will create the necessary folders to correctly create the component.
        * Use case: 
          ```html
          <div aem-component-name="header" ...> 
            ...
          </div> 
          ```
        * Use case: 
          ```html
          <div aem-component-name="content/banner" ...>
            ...
          </div> 
          ```
    * aem-component-group: The component group of the created component.
        * E.g: We can have the following element defining one component:
          ```html 
          <div aem-component-path="ui.apps\src\main\content\jcr_root\apps\myproject\components" aem-models-package="com.myproject.core.models"> 
            <div aem-component-name="content/banner" aem-component-group="Banners"> 
            ... 
            </div> 
          </div>
          ```
* Properties: Defines all the properties of the component.
    * aem-property: The name of the specified property in the dialog and model.
      * Use case: 
        ```html
            <div aem-property="title" ...></div> 
        ```
    * aem-property-label: Label in the dialog of the specified property.
      * Use case:
        ```html
          <div aem-property-label="Title" ...></div> 
        ```
    * aem-property-value: Default shown value when the property is empty.
      * Use case:
        ```html
            <div aem-property-value="My Title" ...></div> 
        ```
    * aem-property-options: For "aem-property-type" that has value "select", the options that will be shown.
        * Use case:
          ```html
            <div aem-property-options="Right,Center,Left" ...></div> 
          ```
    * aem-property-type: The type of the specified property in the dialog input. The following list shows all possible cases:
        * autocomplete
        * checkbox
        * colorfield
        * datepicker
        * fileupload
        * hidden
        * numberfield
        * password
        * pathbrowser
        * pathfield
        * switch
        * textarea
        * textfield
        * multifield
        * select
    * Use cases:
      ```html
      <p aem-property="text" aem-property-label="Text" aem-property-value="My text" aem-property-type="texfield">Lorem ipsum</p> 

      <span aem-property="language" aem-property-label="Language" aem-property-value="Spanish" aem-property-type="select" aem-property-options="Spanish,English,German">Spanish<span> 
      ```

    * Attribute definitions: The following syntaxis is to specify an attribute in HTML, beeing "nameAttr" the name of the attribute we want to configure
      * aem-attribute-nameAttr-name: The name of the specified attribute in the dialog and model.
        * Use case:
          ```html
            <div aem-attribute-href-name="hrefOfMyLink" ...></div> 
          ```
      * aem-attribute-nameAttr-label: Label in the dialog of the specified attribute.
        * Use case:
          ```html
            <div aem-attribute-href-label="Link" ...></div> 
          ```
      * aem-attribute-nameAttr-value: Default shown value when the attribute is empty.
        * Use case:
          ```html
            <div aem-attribute-href-value="#" ...></div> 
          ```
      * aem-attribute-nameAttr-type: The type of the specified attribute in the dialog input. The following list shows all possible cases:
          * autocomplete
          * checkbox
          * colorfield
          * datepicker
          * fileupload
          * hidden
          * numberfield
          * password
          * pathbrowser
          * pathfield
          * switch
          * textarea
          * textfield
          * multifield
          * select
      * Use cases:
      ```html
       <a aem-attribute-href-name="theHrefOfMyComp" aem-attribute-href-label="Href Label" aem-attribute-href-type="textfield" aem-attribute-href-value="#" ></a> 


      <img aem-attribute-src-name="theSrcOfMyComp2" aem-attribute-src-label="test" aem-attribute-src-type="textfield" aem-attribute-src-value="#"/>
      ```
      The previous two cases will let you configure the href and src HTML attribute from the dialog.


* Special uses:
    * Select: For select widgets to specify options you must use aem-property-options, if not specified, the widget will not be created.
    * Multifield: The children elements of the specified multifield will be the shown widgets when the multifield loads.
      * Use case: 
        ```html
        <ul aem-property="myMultifield" aem-property-type="multifield" aem-property-label="My Multifield"> 
          <li aem-property="title" aem-property-type="textfield" aem-property-label="Title">Title</li> 
          <li aem-property="description" aem-property-type="textfield" aem-property-label="Description">Description</li> 
        </ul>
        ```
        Properties "title" and "description" will be created as properties inside the multifield "myMultifield". 
### Code Snippets

In relative path
```html
<div aem-component-path="ui.apps\src\main\content\jcr_root\apps\myProject\components\new-components" 
aem-models-package="com.codebay.myproject.core.models.newcomponents"> 
 <div aem-component-name="books/infographicBook" aem-component-group="My Project - Structure"> 
   <h1 class="test-for class attr" aem-property="title" 
     aem-property-type="textarea" aem-property-value="Default title value" aem-property-label="Preview title"> 
     <div aem-property="subTitle" aem-property-type="textfield" aem-property-value="Default subtitle value"> 
       <a aem-attribute-href-name="theHrefOfMyComp" aem-attribute-href-label="Href Label" aem-attribute-href-type="textfield" aem-attribute-href-value="#" aem-property="subTitleColoring" aem-property-type="colorfield" aem-property-label="Subtitle Coloring"></a> 
     </div>
     <img aem-attribute-src-name="theSrcOfMyComp2" aem-attribute-src-label="test" aem-attribute-src-type="textfield" aem-attribute-src-value="#"/>
     <p aem-property="upperTitle" aem-property-type="textarea" aem-property-label="Upper title"></p> 
   </h1>
   <p aem-property="number" aem-property-type="numberfield"></p> 
   <ul aem-property="myMultifield" aem-property-type="multifield" aem-property-label="My Multifield"> 
     <li aem-property="title" aem-property-type="textfield" aem-property-label="Property 1">Title</li> 
     <li aem-property="description" aem-property-type="select" aem-property-options="option1,option2" 
       aem-property-label="Description">Lorem ipsum</li> 
   </ul> 
   <li aem-property="mySelect" aem-property-type="select" aem-property-options="option1,option2,option3" 
     aem-property-label="Select Example"></li> 
 </div> 
 <div aem-component-name="authorInfo" aem-component-group="My Project - Structure" 
   aem-models-package="com.codebay.myproject.core.models"> 
   <h1 class="class1 class2" aem-property-attribute-title="name" aem-property="name" 
     aem-property-type="textfield" aem-property-value="Default Name value" aem-property-label="Name"> 
   </h1> 
   <div aem-property="surname" aem-property-type="textfield" aem-property-value="Default Surname value"> 
      <span aem-property="favouritecolor" aem-property-type="colorfield" aem-property-label="Favourite Coloring"></span> 
   </div> 
   <p aem-property="age" aem-property-type="numberfield" aem-property-label="Numberfield"></p> 
 </div> 
</div> 
 
```
With an absolute path
```html
<div aem-component-path="\c:\Users\my-user\Desktop" aem-models-package="com.codebay.project2.models"> 
 <div aem-component-name="component1" aem-component-group="My group - My Group"> 
   <h1 class="class1" aem-property-attribute-title="Preview" aem-property="title" 
     aem-property-type="textarea" aem-property-value="Default title value" aem-property-label="Preview title"> 
     <div aem-property="subTitle" aem-property-type="textfield" aem-property-value="Default subtitle value"> 
       <img aem-attribute-src-name="theSrcOfMyComp" aem-attribute-src-label="test" aem-attribute-src-type="textfield" aem-attribute-src-value="#" aem-property="subTitleColoring" aem-property-type="colorfield" aem-property-label="Subtitle Coloring"/> 
     </div>
     <a aem-attribute-href-name="theHrefOfMyComp2" aem-attribute-href-label="Href test" aem-attribute-href-type="textfield" aem-attribute-href-value="#"></a>
     <p aem-property="upperTitle" aem-property-type="textarea" aem-property-label="Upper title"></p> 
   </h1> 
   <p aem-property="text" aem-property-type="numberfield"></p> 
   <ul aem-property="myMultifield" aem-property-type="multifield" aem-property-label="My Multifield"> 
     <li aem-property="property1" aem-property-type="textfield" aem-property-label="Property 1">Title</li> 
     <li aem-property="property2" aem-property-type="select" aem-property-options="option1,option2" 
       aem-property-label="Property 2">Property 2</li> 
   </ul> 
   <li aem-property="mySelect" aem-property-type="select" aem-property-options="option1,option2"></li> 
 </div> 
</div>

```
NOTE: You can add various components of various paths at the same time.
