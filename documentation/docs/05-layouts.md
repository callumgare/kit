---
title: Layouts
---

So far, we've treated pages as entirely standalone components — upon navigation, the existing component will be destroyed, and a new one will take its place.

But in many apps, there are elements that should be visible on _every_ page, such as top-level navigation or a footer. Instead of repeating them in every page, we can use _layout_ components.

To create a layout component that applies to every page, make a file called `src/routes/$layout.svelte`. The default layout component (the one that SvelteKit uses if you don't bring your own) looks like this...

```html
<slot></slot>
```

...but we can add whatever markup, styles and behaviour we want. For example, let's add a nav bar:

```html
<!-- src/routes/$layout.svelte -->
<nav>
	<a href=".">Home</a>
	<a href="about">About</a>
	<a href="settings">Settings</a>
</nav>

<slot></slot>
```

If we create pages for `/`, `/about` and `/settings`...

```html
<!-- src/routes/index.svelte -->
<h1>Home</h1>
```

```html
<!-- src/routes/about.svelte -->
<h1>About</h1>
```

```html
<!-- src/routes/settings.svelte -->
<h1>Settings</h1>
```

...the nav will always be visible, and clicking between the three pages will only result in the `<h1>` being replaced.

### Nested routes

Suppose we don't just have a single `/settings` page, but instead have nested pages like `/settings/profile` and `/settings/notifications` with a shared submenu (for a real-life example, see [github.com/settings](https://github.com/settings)).

We can create a layout that only applies to pages below `/settings` (while inheriting the root layout with the top-level nav):

```html
<!-- src/routes/settings/$layout.svelte -->
<h1>Settings</h1>

<div class="submenu">
	<a href="/settings/profile">Profile</a>
	<a href="/settings/notifications">Notifications</a>
</div>

<slot></slot>
```

The `page` store from `$app/stores` can be useful for styling:

```diff
+<script>
+	import { page } from '$app/stores';
+</script>
+
 <div class="submenu">
-	<a href="/settings/profile">Profile</a>
-	<a href="/settings/notifications">Notifications</a>
+	<a
+		class:selected={$page.path === "/settings/profile"}
+		href="/settings/profile"
+	>Profile</a>
+
+	<a
+		class:selected={$page.path === "/settings/notifications"}
+		href="/settings/notifications"
+	>Notifications</a>
 </div>
```