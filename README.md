# REST API Application in Java

## Adding libraries 

Note: Any one of the below step mentioned in this section is required.

### Java project ( with Jars ) 
* Download Jersey distribution as zip file [Jersey download](https://jersey.github.io/).
* Copy all JARs downloaded to your `WEB-INF/lib` folder.

### Maven based project
```
<dependencies>
  	<dependency>
  		<groupId>org.glassfish.jersey.containers</groupId>
  		<artifactId>jersey-container-servlet</artifactId>
  		<version>2.28</version>
  	</dependency>
  	<dependency>
  		<groupId>org.glassfish.jersey.core</groupId>
  		<artifactId>jersey-server</artifactId>
  		<version>2.28</version>
  	</dependency>
  	<dependency>
  		<groupId>org.glassfish.jersey.core</groupId>
  		<artifactId>jersey-client</artifactId>
  		<version>2.28</version>
  	</dependency>
  	<dependency>
  		<groupId>org.glassfish.jersey.inject</groupId>
  		<artifactId>jersey-hk2</artifactId>
  		<version>2.28</version>
  	</dependency>
  	<dependency>
  		<groupId>javax.xml.bind</groupId>
  		<artifactId>jaxb-api</artifactId>
  		<version>2.3.1</version>
  	</dependency>
  	<dependency>
  		<groupId>org.glassfish.jersey.media</groupId>
  		<artifactId>jersey-media-json-jackson</artifactId>
  		<version>2.28</version>
  	</dependency>
  </dependencies>
```

### Gradle based project
```
dependencies {
    testCompile 'junit:junit:4.11'
    testCompile 'org.hamcrest:hamcrest-all:1.3'
   +-------------------- ========= GOLDEN ======== -------------------------+
   | compile 'org.glassfish.jersey.containers:jersey-container-servlet:2.14'|
   +------------------------------------------------------------------------+
}
```

## Servlet entry to web descriptor ( web.xml ) 
```
<servlet>
  <servlet-name>Jersey REST Service</servlet-name>
  <servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>
  <init-param>
    <param-name>jersey.config.server.provider.packages</param-name>
    <param-value>in.example.restpackage</param-value>          
  </init-param>

  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>Jersey REST Service</servlet-name>
  <url-pattern>/myrestapi/*</url-pattern>
</servlet-mapping>
```

**Variables in snippet of code**
* in.example.restpackage - Package path of your REST controller.
* /myrestapi/* - Prefix in your REST API URL.

## REST Controller
```
package in.example.restpackage;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.core.Response;

@Path("/hello")
public class HelloWorldService {

  /*
    Returning string with status code.
  */
  @GET
  @Path("/{param}")
  public Response getMsg(@PathParam("param") String msg) {

    String output = "Jersey say : " + msg;

    return Response.status(200).entity(output).build();
  }
  
  /*
    Returning object with status code.
  */
  @GET
  @Produces({MediaType.APPLICATION_JSON})
  @Path("/GetHospitalById/{hospitalId}")
  public Response getHospitalByHospitalId(@PathParam(value = "hospitalId") String hospitalId) {
		
    if( null == hospitalId || hospitalId.isEmpty() ){
      throw new RuntimeException("Hospital id should not be empty or null");
    }
    
    Hospital hospital = hospitalService.getHospitalById(hospitalId);
		
    return Response.status(201).entity(hospital).build();		
  }

  
  /*
    Return object directly as response.
  */
  @Path("/users") 
  public List<User> getUsers(){ 
    return new ArrayList<User>(); 
  } 
}
```

### Access your application

Access below URL in the browser
`http://localhost:8080/PROJECT_NAME/myrestapi/hello/ajay`
