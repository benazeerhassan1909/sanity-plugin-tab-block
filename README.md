# @multidots/sanity-plugin-tab-block

> A comprehensive tab block plugin for **Sanity Studio** that provides dynamic, customizable tab interfaces with rich content support.


## Features

### 🎯 Core Functionality
- **Multiple Tabs**: Support for multiple tabs per block with validation
- **Rich Content**: Portable Text support with headings, lists, links, and formatting
- **Flexible Layouts**: Horizontal and vertical tab orientations with intelligent mobile adaptation
- **Responsive Design**: Comprehensive responsive behavior across all device sizes
-- **Full ARIA Compliance**: Complete `role`, `aria-selected`, `aria-controls`, `aria-labelledby`, and `tablist` attributes


### 🎨 Customization Options
- **Layout Control**: Choose between horizontal and vertical tab arrangements
- **Advanced Alignment Settings**: Independent control over title, tab titles, content, and vertical alignment
- **Comprehensive Color Theming**: Full color customization for all visual elements
  - Block title color
  - Tab title colors (active/inactive/hover states)
  - Tab background colors (active/inactive states)
  - Content text and background colors
  - Active tab border colors
  - Tab hover colors


## Installation

```bash
npm install @multidots/sanity-plugin-tab-block
```

## Quick Start

### 1. Add the Plugin to Your Sanity Config

```typescript
import {defineConfig} from 'sanity'
import {tabBlockPlugin} from '@multidots/sanity-plugin-tab-block'

export default defineConfig({
  // ... other config
  plugins: [
    tabBlockPlugin(),
    // ... other plugins
  ],
})
```

### 2. Add to Your Schema

```typescript
// In your document schema
export default defineType({
  name: 'page',
  title: 'Page',
  type: 'document',
  fields: [
    defineField({
          name: "tabBlock",
          type: "tabBlock",
    }),
  ],
})
```

### 3. Render in Your Frontend

#### Create a Component Wrapper

For a clean and maintainable frontend implementation, create a dedicated component wrapper first, then use it in your pages. This approach provides better code organization and reusability.

First, create a Tab.tsx component in your components directory:


```typescript
  'use client'

  import { TabBlock as TabBlockComponent } from "@multidots/sanity-plugin-tab-block"
  import { TabBlock } from "@/sanity/types"
  import { PortableText } from 'next-sanity'

  type TabProps = {
      tab: TabBlock
  }

  const Tab = ({ tab }: TabProps) => {
      return (
          <TabBlockComponent
              tab={tab}
              PortableText={(props: any) => <PortableText {...props} />}
          />
      )
  }

  export default Tab
```

#### Render in Your Page Example

Use in Your Page Component

```typescript
import { TabBlockType } from '@/sanity/types'
import Tab from '@/components/Tab'

// Add code to get the page data

export default async function Page({ params }: RouteProps) {
    const { data: page } = await getPage(params);
    return  (
        <>
            {page?.tabBlock && (
                <Tab tab={page?.tabBlock as TabBlockType} />
            )}
        </>
    ) ;
}
            
```


## Configuration Options

### Schema Fields

| Field | Type | Description | Group |
|-------|------|-------------|-------|
| `title` | string | Optional title for the entire tab block | - |
| `tabs` | array | Array of tab objects (1-10 tabs) | Tabs & Content |
| `layout` | string | `'horizontal'` or `'vertical'` | Layout & Alignment |
| `titleAlignment` | string | `'left'`, `'center'`, or `'right'` | Layout & Alignment |
| `tabTitleAlignment` | string | `'left'`, `'center'`, or `'right'` | Layout & Alignment |
| `tabTitleVerticalAlignment` | string | `'start'`, `'center'`, or `'end'` (vertical layout only) | Layout & Alignment |
| `tabContentAlignment` | string | `'left'`, `'center'`, or `'right'` | Layout & Alignment |

### Color Customization

| Color Field | Description | Usage |
|-------------|-------------|-------|
| `titleColor` | Main block title color | Applied to the main title text |
| `tabTitleColor` | Inactive tab title color | Default color for tab buttons |
| `tabTitleHoverColor` | Tab title hover background color | Background color when hovering over tabs |
| `activeTabTitleColor` | Active tab title color | Text color for the currently active tab |
| `activeTabBorderColor` | Active tab border color | Border color indicating the active tab |
| `tabBackgroundColor` | Tab button background color | Default background for tab buttons |
| `activeTabBackgroundColor` | Active tab background color | Background color for the active tab |
| `tabContentColor` | Tab content text color | Text color within tab content areas |
| `tabContentBackgroundColor` | Tab content area background color | Background color for the content area |

### Tab Object Structure

Each tab in the `tabs` array contains:

```typescript
{
  tabTitle: string        // Tab button text (required, 1-50 characters)
  content: PortableText[] // Rich text content (required)
}
```

## Content Features

### Supported Rich Text Elements
- **Headings**: H1, H2, H3, H4
- **Text Formatting**: Bold, italic, underline
- **Lists**: Bulleted and numbered lists
- **Blockquotes**: Quote styling

### Link Configuration
Links support:
- HTTP/HTTPS URLs
- Email addresses (`mailto:`)
- Phone numbers (`tel:`)

## Component Props

### TabBlock Component

```typescript
interface TabBlockProps {
  tab: TabBlockData           // The tab block data from Sanity
  PortableText?: React.ComponentType<{value: unknown}>  // Portable Text renderer
  className?: string          // Additional CSS classes
}
```

### TabBlockData Interface

```typescript
interface TabBlockData {
  title?: string
  tabs?: Array<{
    tabTitle?: string
    content?: unknown[]
    _type?: string
    _key?: string
  }>
  layout?: 'horizontal' | 'vertical'
  titleAlignment?: 'left' | 'center' | 'right'
  tabTitleAlignment?: 'left' | 'center' | 'right'
  tabTitleVerticalAlignment?: 'start' | 'center' | 'end'
  tabContentAlignment?: 'left' | 'center' | 'right'
  // Color properties
  titleColor?: {hex: string}
  tabTitleColor?: {hex: string}
  tabTitleHoverColor?: {hex: string}
  activeTabTitleColor?: {hex: string}
  activeTabBorderColor?: {hex: string}
  tabBackgroundColor?: {hex: string}
  activeTabBackgroundColor?: {hex: string}
  tabContentColor?: {hex: string}
  tabContentBackgroundColor?: {hex: string}
}
```

## Styling and Customization

### Responsive Design
The component automatically adapts to different screen sizes:
- **Desktop & Tablet**: Maintains chosen layout (horizontal/vertical)
- **Mobile**: Vertical tabs convert to horizontal scrollable navigation

### Custom Styling
You can override styles by:
1. Passing a `className` prop to the component
2. Using CSS specificity to override built-in styles
3. Customizing colors through the Sanity interface
4. Targeting specific responsive breakpoints

### CSS Classes
The component generates these CSS classes:
- `.tab-block` - Main container
- `.tab-block-title` - Block title
- `.tab-container` - Tab layout container (`.horizontal` or `.vertical`)
- `.tab-nav` - Tab navigation container
- `.tab-nav-item` - Individual tab buttons (`.active` for current tab)
- `.tab-content` - Content area
- `.tab-panel` - Individual content panels (`.hidden` for inactive panels)

## Screenshots

### Backend Settings: 

https://share.cleanshot.com/9qzKd1JQVqctkHJqQpNL 

![Backend Settings](https://github.com/benazeerhassan1909/sanity-plugin-tab-block/blob/main/public/tabblockbackendsettings.png)

### Frontend Horizontal View: 

https://share.cleanshot.com/nqlbLY0G7jLLjPJstg0k  

![Frontend Horizontal View](https://github.com/benazeerhassan1909/sanity-plugin-tab-block/blob/main/public/frontendsettings.gif)


### Frontend Vertical View: 

https://share.cleanshot.com/52CCqGWfZ2LxwQj7sbft 

![Frontend Vertical View](https://github.com/benazeerhassan1909/sanity-plugin-tab-block/blob/main/public/forntendsettingsvertical.gif)


## Browser Support

- Modern browsers (Chrome, Firefox, Safari, Edge)
- IE11+ (with appropriate polyfills)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Requirements

- **Node.js**: 18 or higher
- **React**: 18 or higher
- **Sanity**: v3
- **TypeScript**: 5.x (for development)

## Development

### Testing in Sanity Studio

See [Testing a plugin in Sanity Studio](https://github.com/sanity-io/plugin-kit#testing-a-plugin-in-sanity-studio) for instructions on how to run this plugin with hotreload in your studio.

## Examples


### Advanced Configuration

```javascript
// Full customization with all available options
{
  _type: 'tabBlock',
  title: 'Service Information',
  layout: 'vertical',
  titleAlignment: 'center',
  tabTitleAlignment: 'left',
  tabTitleVerticalAlignment: 'start',
  tabContentAlignment: 'left',
  titleColor: {hex: '#2D3748'},
  tabTitleColor: {hex: '#4A5568'},
  tabTitleHoverColor: {hex: '#F7FAFC'},
  activeTabTitleColor: {hex: '#3182CE'},
  activeTabBorderColor: {hex: '#3182CE'},
  tabBackgroundColor: {hex: 'transparent'},
  activeTabBackgroundColor: {hex: '#EBF8FF'},
  tabContentColor: {hex: '#2D3748'},
  tabContentBackgroundColor: {hex: '#F7FAFC'},
  tabs: [
    {
      tabTitle: 'Overview',
      content: [
        {
          _type: 'block',
          children: [{text: 'Comprehensive service overview...'}]
        }
      ]
    },
    {
      tabTitle: 'Features',
      content: [
        {
          _type: 'block',
          children: [{text: 'Key features and benefits...'}]
        }
      ]
    }
  ]
}
```

## Troubleshooting

### Common Issues

**Issue**: Tabs not rendering properly
- **Solution**: Ensure you're passing the `PortableText` component prop

**Issue**: Styles not applying
- **Solution**: Check that color values are in the correct `{hex: string}` format

**Issue**: Content not displaying
- **Solution**: Verify that tab content is properly structured Portable Text

### Performance Tips

1. **Tab Limits**: Keep tabs to 10 or fewer for optimal performance and user experience
2. **Content Optimization**: Use lazy loading for heavy content in tabs
3. **Mobile Considerations**: Consider tab content length and title length for mobile users
4. **Color Usage**: Use the built-in color customization rather than custom CSS for better performance
5. **Responsive Testing**: Test on various devices to ensure optimal responsive behavior

## License

[MIT](LICENSE) © Multidots

## Support

For issues and feature requests, please use the [GitHub issues page](https://github.com/your-org/sanity-plugin-tab-block/issues).

---

**Made with ❤️ by [Multidots](https://multidots.com)**