---
layout: post
category : java
tagline: ""
tags : [java, struts, json]
---
{% include JB/setup %}

### Struts2 JSON Request
If you're using struts2 and want to send JSON parameters to the server, You can use the [JSON Plugin](http://struts.apache.org/release/2.3.x/docs/json-plugin.html). And you don't need to extend its "json-default" package. Actually the package just declares a JSON interceptor and a JSON result-type(we won't use it here). Declare the JSON interceptor in your own package and you can starte sending JSON request to your server. Just make sure you read the rules first:

    1. The "content-type" must be "application/json"
    2. The JSON content must be well formed, see json.org for grammar.
    3. Action must have a public "setter" method for fields that must be populated.
    4. Supported types for population are: Primitives (int,long...String), Date, List, Map, Primitive Arrays, Other class (more on this later), and Array of Other class.
    5. Any object in JSON, that is to be populated inside a list, or a map, will be of type Map (mapping from properties to values), any whole number will be of type Long, any decimal number will be of type Double, and any array of type List. 

### Struts2 JSON Response
When you're developing a modern Ajax-based web application, often you need to return JSON responses to an Ajax client. Although the [JSON Plugin](http://struts.apache.org/release/2.3.x/docs/json-plugin.html) we mentioned above has the JSON result-type. It's not the right approach to fit our needs. 
Actually we can write a **simple** JSON result-type by ourself (suppose we store the JSON response in the "jsonModel" model, which can be found in the valueStack):

    public class SimpleJSONResult implements Result {
        private static final long serialVersionUID = 987690849419734258L;
        @Override
        public void execute(ActionInvocation invocation) throws Exception {
            ServletActionContext.getResponse().setContentType("application/json");
            ServletActionContext.getResponse().setCharacterEncoding("utf-8");
            PrintWriter responseStream =
            ServletActionContext.getResponse().getWriter();
            ValueStack valueStack = invocation.getStack();
            Object jsonModel = valueStack.findValue("jsonModel");   
            if(jsonModel==null){
                ServletActionContext.getResponse().setStatus(HttpStatus.NOT_FOUND.value());
                return;
            }
            responseStream.print(jsonModel);
        }
    }
