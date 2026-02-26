# Recipe: Restaurant Reviews

Map your restaurant reviews using star ratings as markers. Each pin links to a review further down the page, and the legend explains the rating scale at a glance.

## Shortcode

```
{{< map source="mesa-restaurants" legend="mesa-restaurants-legend" color="Gold" icon="circle-star-full" >}}
```

## Parameters used

| Parameter | Value | Why |
|---|---|---|
| `color` | `Gold` | Sets gold as the default pin color, so you don't need to repeat it on every entry. |
| `icon` | `circle-star-full` | Sets the filled star as the default icon. Pins only need to specify `icon` when they differ from the default. |
| `legend` | `mesa-restaurants-legend` | Displays a key below the map explaining the three rating tiers. |

## How it works

The three star icons create a natural rating vocabulary:

- **`circle-star-full`** — Loved it. A place you'd recommend without hesitation.
- **`circle-star-half`** — It was fine. Worth a visit but not a destination.
- **`circle-star-empty`** — Underwhelming. Didn't live up to expectations.

All three use `color: "Gold"` so they read as a unified set on the map. Because `circle-star-full` is set as the default icon in the shortcode, you only need to add `icon` to the half and empty entries.

In this example, each pin's `url` points to a `#bookmark` anchor (e.g., `#worth-takeaway`), so clicking a pin name in the popup jumps the reader to your full review further down the page. To set up the anchors, add an `id` attribute to each review's heading in your post:

```html
<h3 id="worth-takeaway">Worth Takeaway</h3>
```

You could also link each pin directly to a standalone blog post reviewing the restaurant (e.g., `url: "/2025/03/worth-takeaway-review"`), which is the more natural approach if you've already written individual reviews. Or link to your existing Yelp or Google review for that restaurant.

The `description` field holds a one-line summary and visit date, visible in the popup without leaving the map.

## Files

- `mesa-restaurants.yml` — Pin data (10 restaurants in Mesa, AZ)
- `mesa-restaurants-legend.yml` — Legend with three rating tiers

Copy both files to your blog's `data/` folder.

## Variations

- **Color-coded by cuisine:** Instead of gold for everything, use different colors per cuisine (e.g., red for Mexican, green for Mediterranean) and add cuisine categories to the legend.
- **Expanded rating scale:** Add `circle-plus` and `circle-minus` alongside the stars for a recommend/avoid layer.
- **Link to external reviews:** Replace the `#bookmark` URLs with links to your Yelp or Google reviews.
