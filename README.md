# nvim-luasnip-emoji

https://github.com/user-attachments/assets/1c216165-2f0a-4d9c-b9a9-f5cbba8a1bab

## Description

A LuaSnip source of emoji snippets provides 💪 **877 emojis** 💪

## Requirements

- [LuaSnip](https://github.com/L3MON4D3/LuaSnip)

## How to install

Lazy.nvim:

```lua
  {
    'L3MON4D3/LuaSnip',
    dependencies = {
      'aiya000/nvim-luasnip-emoji', -- Include this plugin here
    },
    -- ... your configuration
    config = function()
      ls.add_snippets('all', require('luasnip-emoji')) -- Load this plugin's source
    end,
  }
```

Now you can expand emoji snippets like:
- `emoji_+1` → `👍`
- `emoji_yum` → `😋`
- `emoji_smile` → `😄`

https://github.com/user-attachments/assets/199309d0-262f-48f5-911a-0dc601960b4a

## Very elegant combination with nvim-cmp (Optional, but **very very recommended**)

By using [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) and [cmp_luasnip](https://github.com/saadparwaiz1/cmp_luasnip),
you can see emoji list in completion menu.

> [!IMPORTANT]
> Why this is recommended?
> Because the reason that this plugin created is realizing this behavhior!
> See below movie:

https://github.com/user-attachments/assets/1c216165-2f0a-4d9c-b9a9-f5cbba8a1bab

Lazy.nvim:

```lua
  {
    'hrsh7th/nvim-cmp',
    dependencies = {
      'saadparwaiz1/cmp_luasnip',
      'aiya000/nvim-luasnip-emoji', -- Include this plugin here
    },
    config = function()
      local cmp = require('cmp')
      local ls = require('luasnip')

      ---Allows to show actual emojis in completion menu.
      ---For example:
      ---```
      ---emoji_pig   🐷 Snippet
      ---emoji_dog   🐶 Snippet
      ---emoji_cow   🐮 Snippet
      ---emoji_cat   🐱 Snippet
      ---emoji_ram   🐏 Snippet
      ---```
      local function format_entry_to_show_first_candidate(entry, vim_item)
        local snip = entry:get_completion_item()
        if snip.data ~= nil and snip.data.snip_id ~= nil then
          local snippet = luasnip.get_id_snippet(snip.data.snip_id)
          if snippet ~= nil and snippet.dscr ~= nil and snippet.dscr[1] ~= nil then
            vim_item.kind = snippet.dscr[1] .. ' ' .. vim_item.kind
          end
        end
        return vim_item
      end

      -- Add LuaSnip as a completion source
      cmp.setup({
        snippet = {
          expand = function(args)
            ls.lsp_expand(args.body)
          end,
        },
        formatting = {
          format = function(entry, vim_item)
            if entry.source.name == 'luasnip' then
              return format_entry_to_show_first_candidate(entry, vim_item)
            end
            return vim_item
          end,
        },
        sources = cmp.config.sources({
          { name = 'luasnip' },
        }),
      })
    end,
  }
```

## Q&A

- Q: When complete menu is shown, and input `<C-n>` or `<C-p>` in insert-mode, emoji list suddenly hidden. Why?
- A: This is a general problem of `nvim-cmp`. Please add below configuration:

```lua
  {
    'hrsh7th/nvim-cmp',
    -- ...
    config = function()
      local cmp = require('cmp')

      -- ...

      cmp.setup({
        -- ...

        -- **Add these keymaps**
        mapping = cmp.mapping.preset.insert({
          ['<Tab>'] = cmp.mapping(function(fallback)
            if cmp.visible() then
              cmp.select_next_item()
            else
              fallback()
            end
          end, { 'i', 's' }),
          ['<S-Tab>'] = cmp.mapping(function(fallback)
            if cmp.visible() then
              cmp.select_prev_item()
            else
              fallback()
            end
          end, { 'i', 's' }),
        }),

        -- ...
      })

      -- ...
    end
    -- ...
  }
```
