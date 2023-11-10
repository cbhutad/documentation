Properties are the last required piece to understand POM basics. Maven properties are value placeholders, like properties in Ant. Their values are accessible anywhere within a POM by using the notation ${X}, where X is the property. Or they can be used by plugins as default values, for example:

``` xml
<project>
  ...
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
    <!-- Following project.-properties are reserved for Maven in will become elements in a future POM definition. -->
    <!-- Don't start your own properties properties with project. -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> 
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
  </properties>
  ...
</project>
```

They come in five different styles:

- env.X: Prefixing a variable with "env." will return the shell's environment variable. For example, ${env.PATH} contains the PATH environment variable.
Note: While environment variables themselves are case-insensitive on Windows, lookup of properties is case-sensitive. In other words, while the Windows shell returns the same value for %PATH% and %Path%, Maven distinguishes between ${env.PATH} and ${env.Path}. The names of environment variables are normalized to all upper-case for the sake of reliability.
- project.x: A dot (.) notated path in the POM will contain the corresponding element's value. For example: <project><version>1.0</version></project> is accessible via ${project.version}.
- settings.x: A dot (.) notated path in the settings.xml will contain the corresponding element's value. For example: <settings><offline>false</offline></settings> is accessible via ${settings.offline}.
- Java System Properties: All properties accessible via java.lang.System.getProperties() are available as POM properties, such as ${java.home}.
- x: Set within a <properties /> element in the POM. The value of <properties><someVar>value</someVar></properties> may be used as ${someVar}.