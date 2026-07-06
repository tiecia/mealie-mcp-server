[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/rldiao-mealie-mcp-server-badge.png)](https://mseep.ai/app/rldiao-mealie-mcp-server)

# Mealie MCP Server

A comprehensive Model Context Protocol (MCP) server that enables AI assistants to interact with your [Mealie](https://github.com/mealie-recipes/mealie) recipe database through clients like Claude Desktop.

## ✨ Features

### 🍽️ Recipe Management
- **CRUD Operations**: Create, read, update, patch, duplicate, and delete recipes
- **Advanced Search**: Filter by text, categories, tags, and tools with AND/OR logic
- **Image Management**: Upload images or scrape from URLs, including inline as part of `patch_recipe`
- **Asset Uploads**: Attach documents and files to recipes
- **Metadata Tracking**: Mark recipes as made, track last made dates
- **Source URL**: Set/update a recipe's original source URL (`orgURL`) via `patch_recipe`
- **Titled Instruction Steps**: Group/label instruction steps (e.g. "For the crust", "For the filling") via `create_recipe`/`update_recipe`

### 🛒 Shopping Lists
- **List Management**: Create, update, and delete shopping lists
- **Item Operations**: Add, update, check off, and remove items
- **Bulk Operations**: Create, update, or delete multiple items at once
- **Recipe Integration**: Automatically add recipe ingredients to shopping lists

### 🏷️ Organization
- **Categories**: Organize recipes with categories (Breakfast, Dinner, etc.)
- **Tags**: Tag recipes for easy filtering (Quick, Healthy, Family Favorite)
- **Advanced Filtering**: Search and filter with full pagination support
- **Empty Detection**: Find unused categories and tags

### 📅 Meal Planning
- **Meal Plans**: View and manage meal plans
- **Bulk Creation**: Add multiple meals at once
- **Today's Menu**: Quick access to today's planned meals

## 🚀 Quick Start

### Prerequisites

- Python 3.12+
- Running Mealie instance with API key
- Package manager [uv](https://docs.astral.sh/uv/getting-started/installation/)

### Installation

#### Option 1: Using fastmcp (Recommended)

Install the server directly with the `fastmcp` command:

```bash
fastmcp install src/server.py \
  --env-var MEALIE_BASE_URL=https://your-mealie-instance.com \
  --env-var MEALIE_API_KEY=your-mealie-api-key
```

#### Option 2: Manual Configuration

Add the server to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "mealie-mcp-server": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/repo/src",
        "run",
        "server.py"
      ],
      "env": {
        "MEALIE_BASE_URL": "https://your-mealie-instance.com",
        "MEALIE_API_KEY": "your-mealie-api-key"
      }
    }
  }
}
```

Restart Claude Desktop to load the server.

## 📖 Usage Examples

### Recipe Operations

```
"Search for chicken recipes"
"Create a new recipe for pasta carbonara"
"Duplicate my lasagna recipe"
"Mark the meatloaf recipe as made today"
"Upload an image for the chocolate cake recipe"
```

### Shopping Lists

```
"Create a shopping list for this week"
"Add eggs and milk to my shopping list"
"Add all ingredients from the lasagna recipe to my shopping list"
"Check off milk on my shopping list"
"Delete all checked items from my shopping list"
```

### Organization

```
"Show me all my recipe categories"
"Create a new tag called 'Quick Meals'"
"Find all recipes tagged with 'healthy'"
"Show me categories that have no recipes"
```

### Advanced Filtering

```
"Find recipes that have both 'quick' AND 'healthy' tags"
"Search for breakfast recipes containing 'eggs'"
"Show me all vegetarian dinner recipes"
```

### Titled Instruction Steps & Extra Metadata

```
"Create a recipe with a titled 'For the crust' step and a 'For the filling' step"
"Set the source URL for my lasagna recipe to https://example.com/lasagna"
"Update the pie recipe's description and set its image from this URL, in one call"
```

## 🎯 Available Tools

### Recipe Tools (13 operations)
- `get_recipes` - List/search recipes with advanced filtering
- `get_recipe_detailed` - Get complete recipe details
- `get_recipe_concise` - Get recipe summary
- `create_recipe` - Create new recipe (instructions may include per-step titles/summaries)
- `update_recipe` - Update recipe (full replacement; instructions may include per-step titles/summaries)
- `patch_recipe` - Update specific fields only, including source URL (`org_url`) and image (`image_url`)
- `duplicate_recipe` - Clone a recipe
- `mark_recipe_last_made` - Update last made timestamp
- `set_recipe_image_from_url` - Set image from URL
- `upload_recipe_image_file` - Upload image file
- `upload_recipe_asset_file` - Upload document/asset
- `delete_recipe` - Delete recipe

### Shopping List Tools (14 operations)
- `get_shopping_lists` - List all shopping lists
- `create_shopping_list` - Create new list
- `get_shopping_list` - Get list by ID
- `delete_shopping_list` - Delete list
- `add_recipe_to_shopping_list` - Add recipe ingredients
- `remove_recipe_from_shopping_list` - Remove recipe ingredients
- `get_shopping_list_items` - List all items
- `get_shopping_list_item` - Get item by ID
- `create_shopping_list_item` - Create single item
- `create_shopping_list_items_bulk` - Create multiple items
- `update_shopping_list_item` - Update item (preserves fields)
- `update_shopping_list_items_bulk` - Update multiple items
- `delete_shopping_list_item` - Delete single item
- `delete_shopping_list_items_bulk` - Delete multiple items

### Category Tools (7 operations)
- `get_categories` - List/search categories
- `get_empty_categories` - Find unused categories
- `create_category` - Create new category
- `get_category` - Get by ID
- `get_category_by_slug` - Get by slug
- `update_category` - Update category
- `delete_category` - Delete category

### Tag Tools (7 operations)
- `get_tags` - List/search tags
- `get_empty_tags` - Find unused tags
- `create_tag` - Create new tag
- `get_tag` - Get by ID
- `get_tag_by_slug` - Get by slug
- `update_tag` - Update tag
- `delete_tag` - Delete tag

### Meal Plan Tools (4 operations)
- `get_all_mealplans` - List meal plans
- `create_mealplan` - Create meal plan entry
- `create_mealplan_bulk` - Create multiple entries
- `get_todays_mealplan` - Get today's meals

**Total: 45 tools** providing comprehensive Mealie API coverage

## 🔧 Development

### Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd mealie-mcp-server
```

2. Install dependencies:
```bash
uv sync
```

3. Configure environment:
```bash
cp .env.template .env
# Edit .env with your Mealie instance details
```

4. Run MCP inspector for testing:
```bash
uv run mcp dev src/server.py
```

### Project Structure

```
mealie-mcp-server/
├── src/
│   ├── mealie/              # API client mixins
│   │   ├── client.py        # Base HTTP client
│   │   ├── recipe.py        # Recipe operations
│   │   ├── shopping_list.py # Shopping list operations
│   │   ├── categories.py    # Category operations
│   │   ├── tags.py          # Tag operations
│   │   ├── mealplan.py      # Meal plan operations
│   │   └── __init__.py      # MealieFetcher aggregator
│   ├── tools/               # MCP tool definitions
│   │   ├── recipe_tools.py
│   │   ├── shopping_list_tools.py
│   │   ├── categories_tools.py
│   │   ├── tags_tools.py
│   │   ├── mealplan_tools.py
│   │   └── __init__.py
│   ├── models/              # Pydantic models
│   ├── server.py            # MCP server entry point
│   └── prompts.py           # Server prompts
├── CHANGELOG.md             # Version history
└── README.md
```

## 📚 Important Notes

### Filtering by Tags/Categories

When filtering recipes, you **must use slugs or UUIDs**, not display names:

✅ **Correct:**
```
"Get recipes with tags=['quick-meals', 'healthy']"
```

❌ **Incorrect:**
```
"Get recipes with tags=['Quick Meals', 'Healthy']"
```

Use `get_tags()` or `get_categories()` first to find the correct slugs.

### Titled/Grouped Instruction Steps

`create_recipe` and `update_recipe` accept each instruction step as either a
plain string, or a dict with a required `"text"` key and optional `"title"`/
`"summary"` keys. This lets you label or group steps (e.g. "For the crust",
"For the filling") the same way the Mealie UI supports:

```python
instructions = [
    "Preheat the oven to 350°F.",
    {"title": "For the crust", "text": "Mix flour, butter, and salt until crumbly."},
    {"title": "For the filling", "text": "Whisk eggs, sugar, and vanilla together."},
]
```

Plain strings and titled dicts can be freely mixed in the same list; existing
callers passing only strings continue to work unchanged.

### Setting Source URL and Image via `patch_recipe`

`patch_recipe` now also accepts `org_url` (maps to Mealie's `orgURL` field, the
link back to the recipe's original source) and `image_url` (scrapes and sets
the recipe's image, same as `set_recipe_image_from_url`) so both can be
updated alongside other fields in a single call:

```python
patch_recipe(
    slug="chocolate-chewies",
    org_url="https://www.example.com/recipes/chocolate-chewies",
    image_url="https://www.example.com/images/chocolate-chewies.jpg",
)
```

### Field Preservation

When updating shopping list items, the server automatically preserves all existing fields. You only need to specify the fields you want to change:

```
# Only updates 'checked' field, preserves note, quantity, etc.
update_shopping_list_item(item_id="...", checked=True)
```

## 🐛 Known Issues

None currently! All features have been tested end-to-end with Claude Desktop.

## 🔄 Changelog

See [CHANGELOG.md](CHANGELOG.md) for a detailed list of changes and version history.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Credits

- Based on the original [mealie-mcp-server](https://github.com/rldiao/mealie-mcp-server) by [@rldiao](https://github.com/rldiao)
- [Mealie](https://github.com/mealie-recipes/mealie) - The recipe management system
- [FastMCP](https://github.com/jlowin/fastmcp) - The MCP framework

## 📞 Support

For issues and questions:
- Check the [CHANGELOG.md](CHANGELOG.md) for recent updates
- Review the Mealie API documentation
- Open an issue on GitHub

## 🔗 Related Links

- [Mealie Documentation](https://docs.mealie.io)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Claude Desktop](https://claude.ai/download)
