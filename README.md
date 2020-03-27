# **ASP.NET Identity Management**
Covering topics like identity management, authentication, authorization, statefull & stateless session, opaque tokens, jwt, token based security, bearer tokens, oauth2.0, oidc
# Content
* ASP.NET MVC 5
	* Overview
  * Identity management
* ASP.NET Web API
  * Overview
  * Identity management
## ASP.NET MVC5 Overview
---
Model, View, Controller

Namespaces: System.Web.\*, System.Web.MVC, System.Web.MVC.\*

URL: base/controller/action/parameters

App_Start configrurations such as: Bundle, Filter, Routing

**Controller:**

	Controller/ControllerBase -> ActionResult

	Properties:  ValidateRequest, ViewBag, User, Session, Server, Request, Response, HttpContext, ControllerContext

	Methods: Validate, ActionResults

	Controller to View passing data:
		* ViewBag, ViewData, TempData
		* Model instance created in ActionResult, passed as View() parameter, in view @model app.Models.Person specified, in rest of the page we use Model.propertiest/methods


**View:**

	*.cshtml, razor syntax, @ & @{ ... }

	@helper, @model, @section name{ ... }
	
	Properties: Html, Ajax, ViewBag, ViewContext, Session, User, Context, Request, Response, Server, Cache, Layout
	
	Methods: RenderBody(), RenderSection(), RenderPage(), @Styles.Render("bunde url"), @Scripts.Render("bunde url")

	htmlAttributes in @Html/Ajax helpers is anonymous object, like: new { id = "user" }

Controller & Action attributes: AllowAnonymous, Authorize, HttpGet/Delete/Head/Options/Patch/Post/Put, NonAction, ValidateAntiForgeryToken, Route

**Actions:**

	[HttpGet/Post/Put/Delete] attributes to specify method

	Results: redirect, partial, json, javascript, file, content, view, empty, httpstatuscode

	Actions can return types other than ActionResult, like string.

	Action parameters passed as:
		1. URL query string parameters
		2. Body: form-data, x-www-form-urlencoded, raw
	Action method parameter can be custom type, request parameters should match properties

**Helpers:**

Html:

	Methods: Action, ActionLink, RouteLink, BeginForm, EndForm, 		AntiForgeryToken, Display, Label, Input related methods, Validation related methods

	Sample form:
	@using (Html.BeginForm("action","controller", FormMethod.Post))
	{
		@Html.TextBox("tb1")
		<button type="submit"/>
	}

Ajax:

	Methods: ActionLink, RouteLink, BeginForm

	BeginForm in Ajax is similar to Html variation, with addition of AjaxOptions object parameter.

	AjaxOptions provides additional options such as, element to update & callback functions.
	

Session:

	Controller & View property, useful for session management.

	Browser gets ASP.NET_SessionId session cookie.

	Can be accessed as indexed type, storing useful session information in value.

	Properties and methods are focused on session & it's collection of key/value pairs.