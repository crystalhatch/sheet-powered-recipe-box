# Sheet-Powered Recipe Box

A recipe website that runs off a Google Sheet. The Sheet is the database, one HTML file is the display, and there's no app, server or build step. Add a row to the Sheet and it shows up on the site within a few minutes.

## Why I built it

Deciding what to make for myself, my partner and the kids had turned into daily decision fatigue. I had dozens of healthy recipes I loved and Instagram saves I wanted to try, but they were scattered across screenshots, my mom's notes and photos of cocktail book pages, so I kept making the same few meals.

My first version was a recipe page with the recipes written inside the HTML. Every new recipe meant editing a file, rebuilding a 110 KB page and pasting it into my website by hand, so the list never got used to decide dinner. The fix was to stop letting the page hold the data.

## How it works

```
Google Sheet (Recipes tab)
        |
        |  File > Share > Publish to web > CSV
        v
Published CSV link  --->  index.html fetches it on load and renders the cards
```

- **One home per recipe.** Every recipe is one row in the Sheet. Nothing is copied into the page.
- **The page only handles the look.** Search, category tabs, favorites, kids' picks, seasons, a "want to try" list, ingredient checkboxes and copy buttons. My page went from 110 KB to 26 KB.
- **Private notes stay private.** Keep sources and personal notes on a second tab and publish only the Recipes tab.
- **Fast repeat visits.** The page caches the last copy in the browser, shows it right away, then refreshes from the Sheet.

## Set it up (about 10 minutes)

1. **Make the Sheet.** Create a Google Sheet and name the first tab `Recipes`. Import `sample-recipes.csv` (File > Import > Upload) to get the columns and a few example rows.
2. **Set every cell to plain text.** Select all, then Format > Number > Plain text. Otherwise Google turns "1/2 cup" into a date.
3. **Publish only the Recipes tab.** File > Share > Publish to web. Pick the `Recipes` tab and `Comma-separated values (.csv)`. Under "Published content & settings," uncheck everything except `Recipes`. Click Publish and copy the link.
4. **Paste the link into the page.** In `index.html`, replace `PASTE_YOUR_PUBLISHED_CSV_LINK_HERE` with your link.
5. **Host it anywhere.** Paste the whole file into an HTML embed (Wix, Squarespace, WordPress), or put it on GitHub Pages or Netlify. Published Google Sheets CSV links can be read from any website, so it works from any host.

After that, you only ever edit the Sheet. Google republishes about every 5 minutes.

## The columns

| Column | What goes in it |
|---|---|
| Name | Recipe name |
| Type | Dinner, Soup, Salad, Side, Dressing, Bread, Snack, Dessert, Drink, Cocktail or Temp |
| Favorite | `Yes` or blank |
| Kids | `Yes` or blank |
| Want to try | `Yes` for anything you haven't made yet |
| Seasons | Comma list: Summer, Fall, Winter, Year-round |
| Also shown on | Extra tab to also show it on, like `Snacks` or `Temps` |
| Description | One sentence for the card |
| Ingredients | One per line. A line starting with `## ` is a section header. |
| Steps | One per line. Start each with a bold label: `**Sauce.** Mix...` |
| Facts | One per line as `Label: value`, like `Oven: 425°F, 30 min` |
| Note | A short tip shown on the card |
| Created by | Credit line, used for cocktails |
| ID | Unique kebab-case ID, like `green-chili`. Used for links. |

Tabs on the page: All, Favorites, Dinner, Snacks, Dessert, Kids, Cocktails, Temps, Want to try. Snack, Dessert, Cocktail and Temp get their own tabs; every other type shows under Dinner.

**Two shorthands work inside any cell:**
- `**text**` shows as bold.
- `[[id|Shown text]]` links to another recipe, like `[[garlic-bread|Crusty Garlic Bread]]`.

## Things that would have broken

- **Fractions turning into dates.** Google Sheets reads `1/2` as a date unless the cells are plain text. Set that before you paste anything in.
- **Publishing private notes.** "Publish to web" defaults to the entire document. Make sure only the Recipes tab is selected, and check that your private tab's link doesn't load.
- **Links and bold in plain cells.** Sheets cells can't hold formatting the page can read, hence the shorthands above.
- **"Live" means within minutes.** Google refreshes published sheets on a short delay, so a new row takes a few minutes to appear.

## Adding recipes from my phone

I pair this with a Claude skill that knows the column layout and adds a row to the Sheet through the Google Sheets connector. I send a screenshot or a dictated recipe from my phone, and it lands on the site a few minutes later. Any tool that can append a row to a Google Sheet works the same way, including Google Forms or a phone shortcut.

## When to outgrow it

This is the step before a real app. If it ever needs logins, editing from the page itself or more than a few hundred rows, the Sheet becomes the seed database for a proper build.

## License

MIT. Use it for your own recipe box, your workout library or anything else that's really just a list you want to browse.
