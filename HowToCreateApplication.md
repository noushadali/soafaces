## Example of creating a soafaces application Bundle ##
This example shows how to build a Bundle application. The application uses an input JavaBean to allow users to customize the application behavior. The JavaBean is persisted automatically by the framework.

Follow these steps to learn how to develop, package and deploy your own soafaces Bundle (Bundle) application:

  1. Implement Weblet Interface
  1. Define an Input JavaBean for Weblet (Optional)
  1. Create MANIFEST.MF File
  1. Package Contents into a Bundle JAR file
  1. Deploy the new Bundle to a soafaces Container (like JobServer)

### Step 1: Implement Weblet Interface ###

The Weblet is the primary GUI interface of the application. Here is what the Weblet implementation might look like:

```
public class HelloWorldApp extends SimplePOJOWeblet
{ 
  private TextBox _oNameField = new TextBox();
  private HelloWorldInput _oMyInputBean = null;

  /**
   */
  public HelloWorldApp() {}

  protected void populateMainPanelPOJO(IsSerializable inputBean)
  {
    _oMyInputBean = (HelloWorldInput) inputBean;

    VerticalPanel vSpacer = null;

    getMainPanel().add(new Label("Simple hello world example. Constructs a simple " +
                                  "hello world message"));
    vSpacer = new VerticalPanel(); vSpacer.setHeight("20");
    getMainPanel().add(vSpacer);
    
    getMainPanel().add(new Label("(Enter your name to put into hello world message)"));	
    vSpacer = new VerticalPanel(); vSpacer.setHeight("2");
    getMainPanel().add(vSpacer);
    getMainPanel().add(new Label("Name: "));
    getMainPanel().add(_oNameField);
    vSpacer = new VerticalPanel(); vSpacer.setHeight("20");
    getMainPanel().add(vSpacer);
      
    //Initialize the field to the value in the input JavaBean
    _oNameField.setText(_oMyInputBean.getPersonName());
  }

  /**
   * This method should be used to save the state 
   * of the GUI to the input JavaBean and perform
   * any necessary validation checks. If the validation
   * is not proper it should return false.
   *
   * The Container hosting the Weblet will call
   * this method in order to save the input JavaBean
   * to its persistent store.
   */
  public void onSaveInputBean(SuccessFailCallback callback)
  {
    try {
      //Save the changes made by the user/gui to the input JavaBean
      //person name
      if(_oNameField.getText() == null || _oNameField.getText().trim().equals(""))
      {
        Window.alert("Error: No name specified");
        
        callback.returnFailure();
        return;
      }
      else
      {
        _oMyInputBean.setPersonName(_oNameField.getText());
      }

      callback.returnSuccess();
    }catch(Throwable ex) {
      callback.returnFailure();
    }
  }
}
```


### Step 2: Define an Input JavaBean for Weblet (Optional) ###

The input JavaBean can be edited by the Weblet and used as input and to persist data for use by the application:

```
public class HelloWorldInput implements IsSerializable
{
  private String personName="";

  public HelloWorldInput()
  {
    super();
  }

  /**
   * Name of person to put in the Hello world sentence.
   */
  public void setPersonName(String value)
  {
    personName=value;
  } 

  public String getPersonName()
  {
    return personName;
  } 	
}
```


### Step 3: Create MANIFEST.MF File ###

The MANIFEST.MF file contains the meta information that informs a soafaces container like JobServer what is contained in the Bundle JAR file and what interfaces are defined by the Bundle. The file is placed in the META-INF directory within the Bundle JAR.

```
SOAFaces-Name: Hello World App
SOAFaces-SymbolicName: surda-beansoup-helloworldapp
SOAFaces-Version: 1.0.0
SOAFaces-Description: Simple Weblet/Application with an input JavaBean
SOAFaces-Vendor: Grand Logic, Inc
SOAFaces-Category: example
#Optional input JavaBean used by the Weblet for persistence
SOAFaces-InputBean: org.beansoup.helloworld.client.HelloWorldInput
#GWT client Module package name
SOAFaces-Weblet: org.beansoup.helloworld.Bundle
```

### Step 4: Package Contents into a Bundle JAR file ###

All the code and content are packaged and put into a Bundle JAR file with the file extension .sfb. Here is an example directory structure of the JAR file.

```
META-INF/MANIFEST.MF
classes/org/beansoup/helloworld/client/
                                       HelloWorldApp.java
                                       HelloWorldApp.class
                                       HelloWorldInput.java
                                       HelloWorldInput.class
                                                                                
lib/
    someThirdPartyGWT-Client.jar
```

Here is an example of a simple ANT target that can construct the Bundle archive file:

```
    <target name="build_sfb" depends="compile" description="Package application into a Bundle jar">
    <mkdir dir="build/sfb_build/org-beansoup-helloworld-v1-0-0/classes" />
      <copy todir="build/sfb_build/org-beansoup-helloworld-v1-0-0/classes"
            overwrite="false">
         <fileset dir="${build.classes.dir}"
                  includes="org/beansoup/helloworld/**/*.class"/>
                 
         <fileset dir="${src.dir}"
                  includes="org/beansoup/helloworld/*.xml
                            org/beansoup/helloworld/client/*.java
                            org/beansoup/helloworld/public/*.css"/>
      </copy>
      <jar destfile="build/sfb_build/org-beansoup-helloworld-v1-0-0.sfb"
           basedir="build/sfb_build/org-beansoup-helloworld-v1-0-0"
           includes="**">
    
         <manifest>
           <attribute name="SOAFaces-Name"            value="Hello World App"/>
           <attribute name="SOAFaces-SymbolicName"    value="surda-beansoup-helloworld"/>
           <attribute name="SOAFaces-Version"         value="1.0.0"/>
           <attribute name="SOAFaces-Description"     value="Simple Weblet/Application with an input JavaBean"/>
           <attribute name="SOAFaces-Vendor"          value="Grand Logic, Inc"/>
           <attribute name="SOAFaces-Category"        value="example"/>
           <attribute name="SOAFaces-InputBean"       value="org.beansoup.helloworld3.client.HelloWorldInput"/>
           <attribute name="SOAFaces-Weblet"          value="org.beansoup.helloworld3.client.Bundle"/>
         </manifest>    
      </jar>
```


Notice that the client GWT code requires the source also be included in the JAR file. The GWT compiler requires the source and will use this to compile, on the fly, the Weblet into GWT AJAX code. The lib directory can contain any needed third party JARs used by the Weblet.

Note, that you have the option to include the GWT client code (java source code an compiled code) in the Bundle and let the soafaces container compile the GWT client code into javascript for you (frees developer from dealing with GWT compiler) or you can compile the GWT client code yourself (using the GWT compiler) and include the javascript in the Bundle directly. You simple include the compiled GWT client code in the ```
www``` directory in the Bundle jar.

### Step 5: Deploy the new Bundle to a soafaces Container (like JobServer) ###

Name the Bundle JAR something like myhelloworld.sfb and place it in the JobServer soafaces directory. The name must be unique within JobServer. Now go to the JobServer soafaces Repository tool and you should see the new Bundle. You are now ready to deploy and run your application.