# Razor Pages

hx-post, hx-target
```html
<div>
    <div id="count">
        <partial name="_Count" />
    </div>
    <button hx-target="#count" hx-post="/Increment">Increment</button>
</div>
```

hx-post, hx-swap, hx-swap-oob
```html
<div class="text-center">
    <form hx-swap="beforeend" hx-target="#people" hx-post="/Submit">
        First name: <input type="text" name="FirstName"/>
        Last name: <input type="text" name="LastName"/>
        <hr>
        <button type="submit">Submit</button>
    </form>
    <div id="people">
        <partial name="_People"/>
    </div>
    <div id="people" hx-swap-oob="true">
        <partial name="_Person"/>
    </div>
</div>
```

hx-delete
```html
<p hx-target="closest p" hx-swap="outerHTML" hx-delete=@("/Home/Delete/" + ViewBag.Person.FirstName)>@ViewBag.Person.FirstName @ViewBag.Person.LastName</p>
```