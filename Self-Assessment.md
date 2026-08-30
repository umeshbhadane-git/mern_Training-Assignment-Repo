# Self-Assessment

## 1. What is the difference between 301 and 302?

Both of these are from the 3XX Status code family that stands for Redirection Request.

**301 - Moved Permanently**

This means the resource has permanently moved to another URL.

**302 - Found / Temporary Redirect**

This means the resource is temporarily available at another URL.

## 2. Why is PUT idempotent but POST is not?

Idempotency means performing the same operation multiple times has the same intended final effect as performing it once.

### PUT

Suppose you have a user:

```
PUT /users/101
```

Request:

```json
{
  "name": "Umesh",
  "age": 25
}
```

The server sets user 101 to that representation.

If you send it once:
```
PUT
↓
User 101 = Umesh, 25
```

Send it again:
```
PUT
↓
User 101 = Umesh, 25
```

Send it 10 times:
```
PUT
↓
User 101 = Umesh, 25

PUT
↓
User 101 = Umesh, 25

PUT
↓
User 101 = Umesh, 25
```

The final state is still:

```
User 101
Name = Umesh
Age = 25
```

Therefore, PUT is idempotent.

### POST

Now imagine:

```
POST /users
```

With:

```json
{
  "name": "Umesh"
}
```

The server might create a new user.

First request:
```
POST /users
       ↓
Create user #101
```

Second request:
```
POST /users
       ↓
Create user #102
```

Third:
```
POST /users
       ↓
Create user #103
```

So:

- 1 POST → 1 resource
- 2 POSTs → 2 resources
- 3 POSTs → 3 resources

The result changes every time. Therefore, POST is generally non-idempotent.

## 3. What is the difference between localStorage and sessionStorage?

### LocalStorage

LocalStorage stores data that remains available even after you close and reopen the browser. It remains saved until we explicitly remove it.

### SessionStorage

SessionStorage is similar, but its lifetime is tied to the browser tab/window session. If we create a sessionStorage, we can retrieve it even if we refresh the browser page. However, if we close the browser, the sessionStorage is gone.

## 4. What triggers a reflow vs a repaint?

### Reflow

Reflow happens when something changes that affects the layout or position/size of elements. The browser has to recalculate where elements should be placed.

Example:

```html
<div id="box">Hello</div>
```

Initially:
```
┌──────────────┐
│    Hello     │
└──────────────┘
```

Now JavaScript changes its width:

```javascript
box.style.width = "500px";
```

The browser needs to ask: "If this element is wider, does its position affect other elements?"

Common things that trigger reflow:
- Size
- Position
- Font changes
- Adding or removing elements
- Changing display property

### Repaint

Repaint happens when the appearance of an element changes but its layout doesn't need to change.

For example:

```css
color: red;
```

The element doesn't become bigger or move. The browser basically says: "The element is still in the same place and same size. I just need to redraw how it looks."

## 5. What is CORS and why does it exist?

CORS stands for **Cross-Origin Resource Sharing**. It is a browser security mechanism that allows a server to specify which other origins are permitted to access its resources. It exists because of the browser's Same-Origin Policy, which prevents a website from freely reading resources from another origin.

For example, if a frontend runs on `localhost:3000` and a backend runs on `localhost:8080`, they are different origins. The backend can use CORS response headers to allow the frontend to access its resources.



