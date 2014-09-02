---
layout: post
category : java
tagline: ""
tags : [java, ARM]
---
{% include JB/setup %}

In old times(Before java 7) when you're dealing with resource cleanup, there would be a bunch of boiler-plate code. For example:
    
    ```java
    private static String read(String path){
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(path));
            return reader.readLine();
        } catch (FileNotFoundException e) {
            //do something...
        } catch (IOException e) {
            //do something...
        } finally{
            if(reader != null){
                try {
                    reader.close();
                } catch (IOException e) {
                    //almost nothing can be done here
                }
            }
        }
        return "";
    }
    ```
    
The boiler-plate code sure is annoying and meaningless in most circumstances. So Java 7 solves this problem with new try-with-resources feature:
   
    ```java
    private static String read(String path){
        try(BufferedReader reader = new BufferedReader(new FileReader(path))) {
            return reader.readLine();
        } catch (FileNotFoundException e) {
            //do something...
        } catch (IOException e) {
            //do something...
        }
        return "";
    }    
    ```
    
###Under the hood
If you open the source code of `java.io.BufferedReader`, you will see it implements the `java.lang.AutoCloseable` interface. This interface have only one method: `void close() throws Exception;` and can be implemented on any resource that must be closed when it is no longer needed. The close method is call by JVM immediately after finishing the try block.

If you want to use this feature in your custom resources, just implement the AutoCloseable interface.

