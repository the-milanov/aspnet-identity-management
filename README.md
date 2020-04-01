# **ASP.NET Identity Management**
## Topics:
* ASP.NET MVC 5
  * [Overview](#aspnet-mvc-5-overview)
  * [Identity management](#aspnet-mvc-5-identity-management)
* ASP.NET Web API
  * [Overview](#aspnet-web-api-overview)
  * [Identity management](#aspnet-web-api-identity-management)
## ASP.NET MVC 5 Overview
Model, View, Controller

Namespaces: System.Web.\*, System.Web.MVC, System.Web.MVC.\*

URL: base/controller/action/parameters

App_Start configrurations such as: Bundle, Filter, Routing

**Controllers:**

Controller/ControllerBase -> ActionResult

Properties:  ValidateRequest, ViewBag, User, Session, Server, Request, Response, HttpContext, ControllerContext

Methods: Validate, ActionResults

Controller to View passing data:
	* ViewBag, ViewData, TempData
	* Model instance created in ActionResult, passed as View() parameter, in view @model app.Models.Person specified, in rest of the page we use Model.propertiest/methods

**Views:**

*.cshtml, razor syntax, @ & @{ ... }

@helper, @model, @section name{ ... }

Properties: Html, Ajax, ViewBag, ViewContext, Session, User, Context, Request, Response, Server, Cache, Layout

Methods: RenderBody(), RenderSection(), RenderPage(), @Styles.Render("bunde url"), @Scripts.Render("bunde url")

htmlAttributes in @Html/Ajax helpers is anonymous object, like: new { id = "user" }

**Controller & Action attributes**: AllowAnonymous, Authorize, HttpGet/Delete/Head/Options/Patch/Post/Put, NonAction, ValidateAntiForgeryToken, Route

**Actions:**

[HttpGet/Post/Put/Delete] attributes to specify method

Results: redirect, partial, json, javascript, file, content, view, empty, httpstatuscode

Actions can return types other than ActionResult, like string.

Action parameters passed as:
* URL query string parameters
* Body: form-data, x-www-form-urlencoded, raw
Action method parameter can be custom type, request parameters should match properties

**Helpers:**

Html:

Methods: Action, ActionLink, RouteLink, BeginForm, EndForm, 		AntiForgeryToken, Display, Label, Input related methods, Validation related methods

Sample form:
```
@using (Html.BeginForm("action","controller", FormMethod.Post))
{
	@Html.TextBox("tb1")
	<button type="submit"/>
}
```

Ajax:

Methods: ActionLink, RouteLink, BeginForm

BeginForm in Ajax is similar to Html variation, with addition of AjaxOptions object parameter.

AjaxOptions provides additional options such as, element to update & callback functions.
	

Session:

Controller & View property, useful for session management.

Browser gets ASP.NET_SessionId session cookie.

Can be accessed as indexed type, storing useful session information in value.

Properties and methods are focused on session & it's collection of key/value pairs.

**Forms:**

Forms can be written in plain html:
```	
<form method="post" action="/Home/NameAge">
	<input type="text" name="name" />
	<input type="number" name="age" />
	<input type="submit"/>
</form>
```
With action method taking it looking like:
```
[HttpPost]
public ActionResult NameAge(string name, int age)
{
	ViewBag.Name = name;
	ViewBag.Age = age;
	return View();
}
```
Custom types(Book, Person, ...) can be used as action parameters
In that case, input name attributes should match name of type properties.

In case there are multiply action parameters, input name attribute should match format: parameter.property (person1.name)

Specify what input element values to be binded to parameter:
public ActionResult Index([Bind(Include="Id, Name")]Person){ ... }

@Html.Display/Label/EditorFor(m => m.Age) to do data binding.

Data annotation attributes for model conditions.

Server side validation:
View:
@Html.LabelFor(m => m.FirstName)
@Html.EditorFor(m => m.FirstName)
@Html.ValidationMessageFor(m => m.FirstName)
Controller:
public ActionResult Index() => View();
[HttpPost]
public ActionResult Index(Person guy)
{
	if (ModelState.IsValid)
	{
		return Redirect("OkView");
	}
	return View(guy);
}

**File Download:**

* Download from Content folder, link with href="~/Content/img.jpeg"
* FileResult return type of action

**File Upload:**

HttpPostedFileBase & it's derived types are used as action method parameters when we upload files using forms.

View Html.BeginForm method takes additional parameter: new { enctype = "multipart/form-data" }

In action we call file.SaveAs(@"path") to save file, 

*Server* property can be used to map local to global paths.

## ASP.NET MVC 5 Identity Management

**Basic structure:**

App_Start has IdentityConfig.cs & Startup.Auth.cs files

New common namespaces: Microsoft.AspNet.Identity, Microsoft.AspNet.Identity*, Microsoft.Owin, Microsoft.Owin.Security, Microsoft.Owin.Security*, System.Security.Claims

<u>User</u> property in controller & view can be used to get user data. Main members are Identity & IsInRole(string name).

Request.IsAuthenticated in view to check if user is authenticated.

Controllers have AccountController, for log in/sign up operations.

Models has AccountViewModels & IdentityModels.

Commonly used base types: IdentityUser, IdentityDBContext, UserManager, SignInManager

**Common types:**

IdentityUser, IdentityDBContext, UserManager, SignInManager, IAuthenticationManager are types commonly used in account management. They can be inherited & expanded upon.

IdentityUser contains user account information, like username, email password hash.

IdentityDBContext is EntityFramework database context used for storing user information.

UserManager can create, search and perform other user related operations.

SignInManager can sign in user, in session or if *remember me* checkbox is checked, in persistent way.

IAuthenticationManager can log out users.

**Database:**

Test database is in App_Data folder, *.mdf file. Connection string is in Web.config, and it is used by ApplicationDbContext.

Database consists of next tables: Users, Roles, UserClaims, UserLogins, UserRoles.

**OAuth2.0 & OpenID Connect:**

After obtaining client id & secret, they need to be added in App_Start > Startup.Auth.cs

In MVC this login type is called "External" log in.

**Roles & Claims:**

[Authorize] attribute is used to allow only authenticated users in Controllers/Actions.

[AllowAnonymous] overrides that.

IdentityUserClaim & IdentityUserRole are commonly used.

ApplicationUser/IdentityUser has properties Roles & Claims.

UserManager property can be used to manage claims/roles for specific user.

IdentityDbContext/ApplicationDbContext has Roles table.

RoleManager is for roles, what UserManager is for users.

[Authorize(Users = "...", Roles="...")] attribute parameters can be used to further constraint controller/action access.

## ASP.NET Web API Overview

Built on top of MVC to provide functionality needed for RESTful service development.

App_Start has WebApiConfig.cs that handles api configuration, and api routing, not done by RouteConfig.cs

ApiController is type inherited by application controllers.

BaseUrl/Help provides generated api docs.

Commonly used tools for testing are Postman & Fiddler.

**Actions:**

Action methods names match HTTP method, that is used to access them.

GET/POST/PUT/DELETE api/controller

Action return types can be string, int, IEnumerable\<T> and so on, or types derived from IHttpActionResult, such as Ok(), NotFound(), ...

Route attribute above action method, can specify custom route to access it.

[FromBody] & [FromUri] can be used on action method parameters to specify their origin.

## ASP.NET Web API Identity Management

Help page (/Help) contains api routes, and their descriptions regarding account management. For example api/Account/Register contains information such as http method and parameters used.

**Register:**

/Help api/Account/Register explains how to create account. In Postman, we can make POST request to that endpoint, with required data in body as x-www-form-urlencoded or raw json. If we get response 200 OK, account is created

**Log in / Get token:**

This is not documented on Help page. We obtain token from /token route. POST request with body x-www-form-urlencoded containing: grant_type: password, username: email, password: password.

Response is in Body, it contains json, we need access_token value for making requests towards protected resources.

**Get protected resource:**

Obtaining resources from actions or controllers that have [Authorize] attribute requires access token.

We make GET request towards api/values with Authorization header with value: "Bearer access_token_value"