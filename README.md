# Swashbuckle.AspNetCore.Filters
[![Build status](https://ci.appveyor.com/api/projects/status/l6cowxsqp2n9e4sl?svg=true)](https://ci.appveyor.com/project/mattfrear/swashbuckle-aspnetcore-examples)
[![NuGet](https://img.shields.io/nuget/v/Swashbuckle.AspNetCore.Filters.svg)](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Filters/)

| :mega: Rename to Swashbuckle.AspNetCore.Filters |
|--------------|
| This project was formerly called Swashbuckle.AspNetCore.Examples, but it has grown from there to become a grab-bag of various filters I have created (or copied) since I started used Swashbuckle in 2015. So I have renamed it.|

This library contains a bunch of filters for [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).
- Add example requests https://mattfrear.com/2016/01/25/generating-swagger-example-requests-with-swashbuckle/
- Add example responses https://mattfrear.com/2015/04/21/generating-swagger-example-responses-with-swashbuckle/
- Add security info to each operation that has an `[Authorize]` endpoint, allowing you to send OAuth2 bearer tokens via Swagger-UI https://mattfrear.com/2018/07/21/add-an-authorization-header-to-your-swagger-ui-with-swashbuckle-revisited/
- Add any old request header to all requests
- Add any old response header to all responses
- Add an indicator to each endpoint to show if it has an `[Authorize]` header (and for which policies and roles)

## Table of Contents

- [Where to get it](#where-to-get-it)
- [What's included](#whats-included)
  - [Request example](#request-example) 
  - [Response example](#response-example)
  - [Security requirements filter](#security-requirements-filter)
  - [Add a request header](#add-a-request-header)
  - [Add a response header](#add-a-response-header)
  - [Add Authorization to Summary](#add-authorization-to-summary)
- [Installation](#installation) 
- [How to use](#how-to-use)
  - [How to use - Request examples](#how-to-use---request-examples) 
    - [Automatic annotation](#automatic-annotation)
    - [Manual annotation](#manual-annotation)
      [List Request examples](#list-request-examples)
  - [How to use - Response examples](#how-to-use---response-examples)
    - [Automatic annotation](#automatic-annotation-1)
    - [Manual annotation](#manual-annotation)
    - [Known issues](#known-issues)
  - [How to use - Security requirements filter](#how-to-use---security-requirements-filter)
  - [How to use - Request Header](#how-to-use---request-header)
  - [How to use - Authorization summary](#how-to-use---authorization-summary)
- [Pascal case or Camel case?](#pascal-case-or-camel-case)
- [Render Enums as strings](#render-enums-as-strings)
- [Advanced: Examples with Dependency injection](#advanced-examples-with-dependency-injection)

## Where to get it
From NuGet.

| Version of Swashbuckle you're using | You'll want this version of this package |
|-------------------------------------|-----------------------------------------|
| Swashbuckle 1.0 - 5.5 | https://www.nuget.org/packages/Swashbuckle.Examples/ |
| Swashbuckle.AspNetCore version 1.0.0 - 2.5.0 | https://www.nuget.org/packages/Swashbuckle.AspNetCore.Examples/ |
| Swashbuckle.AspNetCore version 3.0.0  | https://www.nuget.org/packages/Swashbuckle.AspNetCore.Filters/ |
| Swashbuckle.AspNetCore version 4.0.0 and above | https://www.nuget.org/packages/Swashbuckle.AspNetCore.Filters/ Version >= 4.5.1 |
| Swashbuckle.AspNetCore version 5.0.0-beta and above | https://www.nuget.org/packages/Swashbuckle.AspNetCore.Filters/ Version >= 5.0.0-beta |

## What's included
### Request example

Populate swagger's `paths.path.[http-verb].requestBody.content.[content-type].example` with whatever object you like.

This is great for manually testing and demoing your API as it will prepopulate the request with some useful data, so that 
when you click the example request in order to populate the form, instead of getting an autogenerated request like this:

![autogenerated request with crappy data](https://mattfrear.files.wordpress.com/2016/01/untitled.png?w=700)

You’ll get your desired example, with useful valid data, like this:

![custom request with awesome data](https://mattfrear.files.wordpress.com/2016/01/capture2.jpg?w=700)

You can see the example output in the underlying swagger.json file, which you can get to by starting your solution and 
navigating to swagger/v1/swagger.json

![swagger.json](https://mattfrear.files.wordpress.com/2016/01/capture.jpg)

As of version 5.0.0-beta, XML examples are also supported.

### Response example

Allows you to add custom data to the example response shown in Swagger. So instead of seeing the default boring data like so:

![response with crappy data](https://mattfrear.files.wordpress.com/2015/04/response-old.png)

You'll see some more realistic data (or whatever you want):

![response with awesome data](https://mattfrear.files.wordpress.com/2015/04/response-new.png?w=700&h=358)

As of version 5.0.0-beta, XML examples are also supported.

### Security requirements filter

Adds security information to each operation so that you can send an Authorization header to your API. Useful for API endpoints that have JWT token
authentication. e.g.

![authorization button](https://mattfrear.files.wordpress.com/2018/07/authbutton.jpg)

![bearer token](https://mattfrear.files.wordpress.com/2018/07/authbuttonclicked.jpg)

### Add a request header

Adds any string to your request headers for all requests. I use this for adding a correlationId to all requests.
![request header](https://mattfrear.files.wordpress.com/2018/01/header.jpg)

### Add a response header

Allows you to specify response headers for any operation
![response headers](https://user-images.githubusercontent.com/169179/35051682-b8217740-fb9d-11e7-8bec-98d4b088dfa5.png)

### Add Authorization to Summary

If you use the `[Authorize]` attribute  to your controller or to any actions, then (Auth) is added to the action's summary,
along with any specified policies or roles.

![authorization](https://user-images.githubusercontent.com/169179/36599686-1d939bcc-18a8-11e8-8f81-d8706f1f0dc1.JPG)

## Installation
1. Install the [NuGet package](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Filters/)

2. In the _ConfigureServices_ method of _Startup.cs_, inside your `AddSwaggerGen` call, enable whichever filters you need

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc();

    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new Info { Title = "My API", Version = "v1" });
        
        // [SwaggerRequestExample] & [SwaggerResponseExample]
        // version < 3.0 like this: c.OperationFilter<ExamplesOperationFilter>(); 
        // version 3.0 like this: c.AddSwaggerExamples(services.BuildServiceProvider());
        // version > 4.0 like this:
        c.ExampleFilters();
        
        c.OperationFilter<AddHeaderOperationFilter>("correlationId", "Correlation Id for the request", false); // adds any string you like to the request headers - in this case, a correlation id
        c.OperationFilter<AddResponseHeadersFilter>(); // [SwaggerResponseHeader]

        var filePath = Path.Combine(AppContext.BaseDirectory, "WebApi.xml");
        c.IncludeXmlComments(filePath); // standard Swashbuckle functionality, this needs to be before c.OperationFilter<AppendAuthorizeToSummaryOperationFilter>()

        c.OperationFilter<AppendAuthorizeToSummaryOperationFilter>(); // Adds "(Auth)" to the summary so that you can see which endpoints have Authorization
        // or use the generic method, e.g. c.OperationFilter<AppendAuthorizeToSummaryOperationFilter<MyCustomAttribute>>();

        // add Security information to each operation for OAuth2
        c.OperationFilter<SecurityRequirementsOperationFilter>();
        // or use the generic method, e.g. c.OperationFilter<SecurityRequirementsOperationFilter<MyCustomAttribute>>();

        // if you're using the SecurityRequirementsOperationFilter, you also need to tell Swashbuckle you're using OAuth2
        c.AddSecurityDefinition("oauth2", new ApiKeyScheme
        {
            Description = "Standard Authorization header using the Bearer scheme. Example: \"bearer {token}\"",
            In = "header",
            Name = "Authorization",
            Type = "apiKey"
        });
    });
}
```

3. If you want to use the Request and Response example filters (and have called `c.ExampleFilters()` above), then you MUST also call either 
```csharp
    services.AddSwaggerExamplesFromAssemblyOf<MyExample>();
```
This will register your examples with the ServiceProvider.

## How to use

### How to use - Request examples

#### Automatic annotation
Version 4.0 and above supports automatic annotation.

Let's say you have a controller action which takes some input from the body, in this case a `DeliveryOptionsSearchModel`:
```csharp
[HttpPost]
public async Task<IHttpActionResult> DeliveryOptionsForAddress([FromBody]DeliveryOptionsSearchModel search)
```
Then all you need to do is implement `IExamplesProvider<DeliveryOptionsSearchModel>`:
```csharp
public class DeliveryOptionsSearchModelExample : IExamplesProvider<DeliveryOptionsSearchModel>
{
    public DeliveryOptionsSearchModel GetExamples()
    {
        return new DeliveryOptionsSearchModel
        {
            Lang = "en-GB",
            Currency = "GBP",
            Address = new AddressModel
            {
                Address1 = "1 Gwalior Road",
                Locality = "London",
                Country = "GB",
                PostalCode = "SW15 1NP"
            }
        };
    }
```
And that's it.

#### Manual annotation
Alternatively, if you want to be more explicit, you can use the `SwaggerRequestExample` attribute. This is how it was done in versions 1.0 - 3.0. Any manual annotations will override automatic annotations.

Decorate your controller methods with the included `SwaggerRequestExample` attribute:

```csharp
[HttpPost]
[SwaggerRequestExample(typeof(DeliveryOptionsSearchModel), typeof(DeliveryOptionsSearchModelExample))]
public async Task<IHttpActionResult> DeliveryOptionsForAddress([FromBody]DeliveryOptionsSearchModel search)
```

Now implement a `IExamplesProvider<T>`, in this case via a DeliveryOptionsSearchModelExample which will generate the example data. It should return the type you specified when you specified the `[SwaggerRequestExample]`.
	
```csharp
public class DeliveryOptionsSearchModelExample : IExamplesProvider<DeliveryOptionsSearchModel>
{
    public DeliveryOptionsSearchModel GetExamples()
    {
        return new DeliveryOptionsSearchModel
        {
            Lang = "en-GB",
            Currency = "GBP",
            Address = new AddressModel
            {
                Address1 = "1 Gwalior Road",
                Locality = "London",
                Country = "GB",
                PostalCode = "SW15 1NP"
            },
            Items = new[]
            {
                new ItemModel
                {
                    ItemId = "ABCD",
                    ItemType = ItemType.Product,
                    Price = 20,
                    Quantity = 1,
                    RestrictedCountries = new[] { "US" }
                }
            }
        };
    }
```

In the Swagger document, this will populate the operation's requestBody content type example property. https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#mediaTypeObject

Here's what the OpenApi 3.0 spec says about it:

Field Name | Type | Description
---|:---:|---
example | Any | Example of the media type.  The example object SHOULD be in the correct format as specified by the media type.  The `example` object is mutually exclusive of the `examples` object.  Furthermore, if referencing a `schema` which contains an example, the `example` value SHALL _override_ the example provided by the schema.

#### List Request examples
As of version 2.4, `List<T>` request examples are supported. For any `List<T>` in the request, you may define a SwaggerRequestExample for `T`. 
Your IExamplesProvider should only return a single `T` and not a `List<T>`.
Working example:

```csharp
[SwaggerRequestExample(typeof(PeopleRequest), typeof(ListPeopleRequestExample), jsonConverter: typeof(StringEnumConverter))]
public IEnumerable<PersonResponse> GetPersonList([FromBody]List<PeopleRequest> peopleRequest)
{

// and then:

public class ListPeopleRequestExample : IExamplesProvider<PeopleRequest>
{
    public PeopleRequest GetExamples()
    {
        return new PeopleRequest { Title = Title.Mr, Age = 24, FirstName = "Dave in a list", Income = null };
    }
}

```

### How to use - Response examples
#### Automatic annotation
Version 4.0 supports automatic annotation.
If it's obvious which type your action returns, then no `ProducesResponseType` or `SwaggerResponse` attributes need to be specified, e.g.
```csharp
public async Task<ActionResult<IEnumerable<Country>>> Get(string lang)
// or public ActionResult<IEnumerable<Country>> Get(string lang)
// or public IEnumerable<Country> Get(string lang)

```

Or, you can optionally decorate your methods (or controller) with either the `ProducesResponseType` or the `SwaggerResponse` attribute:
```csharp
[SwaggerResponse(200, "The list of countries", typeof(IEnumerable<Country>))]
// or, like this [ProducesResponseType(typeof(IEnumerable<Country>), 200)]
[SwaggerResponse(400, type: typeof(IEnumerable<ErrorResource>))]
public async Task<HttpResponseMessage> Get(string lang)
```
Now you’ll need to add an Examples class, which will implement `IExamplesProvider<T>` to generate the example data for the Response:

```csharp	
public class CountryExamples : IExamplesProvider<IEnumerable<Country>>
{
    public IEnumerable<Country> GetExamples()
    {
        return new List<Country>
        {
            new Country { Code = "AA", Name = "Test Country" },
            new Country { Code = "BB", Name = "And another" }
        };
    }
}
```
#### Manual annotation
Alternatively, if you want to be more explicit, you can use the `SwaggerResponseExample` attribute. This is how it was done in versions 1.0 - 3.0. Any manual annotations will override automatic annotations.

Decorate your methods with the new SwaggerResponseExample attribute:
```csharp
[SwaggerResponse(200, "The list of countries", typeof(IEnumerable<Country>))]
// or, like this [ProducesResponseType(typeof(IEnumerable<Country>), 200)]
[SwaggerResponseExample(200, typeof(CountryExamples))]
[SwaggerResponse(400, type: typeof(IEnumerable<ErrorResource>))]
public async Task<HttpResponseMessage> Get(string lang)
```
For manual annotation implement `IExamplesProvider<T>` to generate the example data

```csharp	
public class CountryExamples : IExamplesProvider<List<Country>>
{
    public List<Country> GetExamples()
    {
        return new List<Country>
        {
            new Country { Code = "AA", Name = "Test Country" },
            new Country { Code = "BB", Name = "And another" }
        };
    }
}
```

In the Swagger document, this will populate the operation's responses content [example object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#responseObject).
The spec for this says:

Field Pattern | Type | Description
---|:---:|---
example | Any | Example of the media type.  The example object SHOULD be in the correct format as specified by the media type.  The `example` object is mutually exclusive of the `examples` object.  Furthermore, if referencing a `schema` which contains an example, the `example` value SHALL _override_ the example provided by the schema.

Example response for application/json mimetype of a Pet data type:

```js
 "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Pet"
                },
                "example": "{\r\n  \"name\": \"Lassie\",\r\n  \"type\": \"Dog\",\r\n  \"color\": \"Black\",\r\n  \"gender\": \"Femail\",\r\n  \"breed\": \"Collie\" \r\n}"
              },
```


#### Known issues
- Request examples are not shown for querystring parameters (i.e. HTTP GET requests, or for querystring parameters for POST, PUT etc methods). Request examples will only be shown in request body.
This is as per the OpenApi 3.0 spec.

### How to use - Security requirements filter

First you need to already have OAuth2 configured correctly, and some of your controllers and actions locked down with the `[Authorize]` attribute.

Then you need to tell Swagger that you're using OAuth2, as shown in the Installation section above:
```csharp
    services.AddSwaggerGen(c =>
    {
        c.AddSecurityDefinition("oauth2", new ApiKeyScheme
        {
            Description = "Standard Authorization header using the Bearer scheme. Example: \"bearer {token}\"",
            In = "header",
            Name = "Authorization",
            Type = "apiKey"
        });
```
This adds a securityDefinition to the bottom of the Swagger document, which Swagger-UI renders as an "Authorize" button, which when clicked brings up the Authorize dialog box shown above.

Then, when you enable the SecurityRequirementsOperationFilter:
```csharp
	// add Security information to each operation for OAuth2
	c.OperationFilter<SecurityRequirementsOperationFilter>();
```
It adds a security property to each operation, which renders in Swagger-UI as a padlock next to the operation:
![locked down actions](https://mattfrear.files.wordpress.com/2018/07/securityonaction.jpg)

By default, the SecurityRequirementsOperationFilter also adds 401 and 403 to each operation that has `[Authorize]` on it:
![401 and 403](https://mattfrear.files.wordpress.com/2018/07/401-403.jpg)

If you don't want to do that you can pass false when you configure it:

```csharp
	c.OperationFilter<SecurityRequirementsOperationFilter>(false);
```

### How to use - Request Header
When you enable the filter in your `Startup.cs`, as per the Installation section above, you can specify the name (string)
and description (string) of the new header parameter, as well as whether it is required or not (bool).
This will add the input box to *every* controller action.

### How to use - Response headers

Specify one or more `[SwaggerResponseHeader]` attributes on your controller action, like so:
```csharp
[SwaggerResponseHeader(StatusCodes.Status200OK, "Location", "string", "Location of the newly created resource")]
[SwaggerResponseHeader(200, "ETag", "string", "An ETag of the resource")]
[SwaggerResponseHeader(new int[] { 200, 401, 403, 404 }, "CustomHeader", "string", "A custom header")]
public IHttpActionResult GetPerson(PersonRequest personRequest)
{
```

### How to use - Authorization summary
Specify `[Authorization]` headers on either a Controller:
```csharp
[Authorize]
public class ValuesController : Controller
```
or on an action:
```csharp
[Authorize("Customer")]
public PersonResponse GetPerson([FromBody]PersonRequest personRequest)
```
You can optionally specify policies `[Authorize("Customer")]` or roles `[Authorize(Roles = "Customer")]` and they will be added to the Summary too.

## Pascal case or Camel case?
The default is camelCase. If you want PascalCase you can pass in a `DefaultContractResolver` like so:
`[SwaggerResponseExample(200, typeof(PersonResponseExample), typeof(DefaultContractResolver))]`

## Render Enums as strings
By default `enum`s will output their integer values. If you want to output strings you can pass in a `StringEnumConverter` like so:
`[SwaggerResponseExample(200, typeof(PersonResponseExample), jsonConverter: typeof(StringEnumConverter))]`

## Advanced: Examples with Dependency injection

If for some reason you need to have examples with DI (for example, to read them from a database), you can use constructor injection:

```csharp
internal class PersonRequestExample : IExamplesProvider
{
    private readonly IHostingEnvironment _env;

    public PersonRequestExample(IHostingEnvironment env)
    {
        _env = env;
    }
    public object GetExamples()
    {
        return new PersonRequest { Age = 24, FirstName = _env.IsDevelopment() ? "Development" : "Production", Income = null };
    }
}
```
Then, you should register the Swagger examples via the `FromAssemblyOf<T>` extension method.
```csharp
services.AddSwaggerExamplesFromAssemblyOf<PersonRequestExample>();
```
If you are using `services.AddSwaggerExamples()`, then you would have to manually register your `IExamplesProvider` class:
```csharp
services.AddSingleton<PersonRequestExample>();
```
The `FromAssemblyOf<T>` extension method is the recommended approach.
