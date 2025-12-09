- [[Tech Fundamentals]]

## Overview
- Search Engine Optimization
- Improve visibility in search results
- Organic (unpaid) traffic
- Technical and content optimization

## Core Concepts

### How Search Engines Work
1. **Crawling** - Bots discover pages
2. **Indexing** - Pages stored in search index
3. **Ranking** - Pages ranked by relevance
4. **Serving** - Results shown to users

### Key Factors
- **Relevance** - Content matches query
- **Authority** - Site credibility and trust
- **User Experience** - Page speed, mobile-friendly
- **Technical SEO** - Proper structure and markup

## Technical SEO

### Make Content Crawlable

#### Server-Side Rendering (SSR)
```javascript
// Next.js example
export async function getServerSideProps() {
    return {
        props: {
            data: await fetchData()
        }
    };
}
```

#### Pre-rendering
```javascript
// Static Site Generation (SSG)
export async function getStaticProps() {
    return {
        props: { data },
        revalidate: 3600 // ISR
    };
}
```

#### Client-Side Rendering Issues
- JavaScript must execute for content
- Slower initial load
- May not be fully indexed
- **Solution**: Use SSR/SSG for SEO-critical pages

### robots.txt
```
# Allow all crawlers
User-agent: *
Allow: /

# Disallow specific paths
Disallow: /admin/
Disallow: /private/
Disallow: /*.json$

# Sitemap location
Sitemap: https://example.com/sitemap.xml
```

### Sitemap.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>https://example.com/page1</loc>
        <lastmod>2024-01-15</lastmod>
        <changefreq>weekly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://example.com/page2</loc>
        <lastmod>2024-01-15</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.5</priority>
    </url>
</urlset>
```

**Best Practices:**
- Include all important pages
- Update when content changes
- Submit to Google Search Console
- Keep under 50,000 URLs per sitemap
- Use sitemap index for large sites

## On-Page SEO

### HTML Meta Tags

#### Title Tag
```html
<title>Best Laptops 2024 - Reviews & Buying Guide | TechStore</title>
```
- **Length**: 50-60 characters
- Include primary keyword
- Unique per page
- Brand name at end

#### Meta Description
```html
<meta name="description" content="Find the best laptops in 2024. Expert reviews, comparisons, and buying guides. Free shipping on orders over $100.">
```
- **Length**: 150-160 characters
- Compelling and relevant
- Include call-to-action
- Unique per page

#### Meta Keywords (Deprecated)
- Not used by major search engines
- Can be ignored

#### Open Graph (Social Sharing)
```html
<meta property="og:title" content="Best Laptops 2024">
<meta property="og:description" content="Expert reviews and buying guides">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/laptops">
<meta property="og:type" content="website">
```

#### Twitter Cards
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Best Laptops 2024">
<meta name="twitter:description" content="Expert reviews">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

### Canonical URLs

#### What is a Canonical URL?
A canonical URL tells search engines: **"This is the main/preferred version of this page"**

**The Problem:**
- Same content accessible via multiple URLs
- Search engines see duplicate content
- SEO value gets split across URLs
- Example of duplicate URLs:
  ```
  https://example.com/product/laptop
  https://example.com/product/laptop/
  https://example.com/product/laptop?ref=homepage
  https://www.example.com/product/laptop
  https://example.com/product/laptop#reviews
  ```

**The Solution:**
```html
<!-- On ALL duplicate versions, point to the canonical -->
<link rel="canonical" href="https://example.com/product/laptop">
```

#### How It Works
1. You have multiple URLs showing the same content
2. Add canonical tag to each duplicate pointing to preferred URL
3. Search engines understand which URL to index and rank
4. All SEO value consolidates to canonical URL

#### Example Scenario
```html
<!-- URL: https://example.com/product/laptop -->
<link rel="canonical" href="https://example.com/product/laptop">

<!-- URL: https://example.com/product/laptop?ref=homepage -->
<link rel="canonical" href="https://example.com/product/laptop">

<!-- URL: https://example.com/product/laptop/ -->
<link rel="canonical" href="https://example.com/product/laptop">

<!-- URL: https://www.example.com/product/laptop -->
<link rel="canonical" href="https://example.com/product/laptop">
```

#### Rules
- **Use absolute URLs** (with https://)
- **One canonical per page** (in `<head>`)
- **Self-referencing**: Canonical can point to itself
- **Cross-domain**: Can point to different domain (rare)

#### Common Use Cases

**1. URL Parameters**
```html
<!-- https://example.com/products?category=laptops&sort=price -->
<link rel="canonical" href="https://example.com/products?category=laptops">
```

**2. Trailing Slash**
```html
<!-- https://example.com/about/ -->
<link rel="canonical" href="https://example.com/about">
```

**3. HTTP vs HTTPS**
```html
<!-- http://example.com/page -->
<link rel="canonical" href="https://example.com/page">
```

**4. Pagination**
```html
<!-- https://example.com/blog?page=2 -->
<link rel="canonical" href="https://example.com/blog?page=2">
<!-- Each page canonicalizes to itself -->
```

**5. Mobile vs Desktop**
```html
<!-- Mobile version -->
<link rel="canonical" href="https://example.com/product/laptop">
<!-- Points to desktop version -->
```

#### JavaScript Example (Dynamic)
```javascript
// Set canonical URL dynamically
function setCanonical(url) {
    // Remove existing canonical
    const existing = document.querySelector('link[rel="canonical"]');
    if (existing) {
        existing.remove();
    }
    
    // Add new canonical
    const link = document.createElement('link');
    link.rel = 'canonical';
    link.href = url;
    document.head.appendChild(link);
}

// Usage
setCanonical('https://example.com/product/laptop');
```

#### Best Practices
- Always use canonical tags
- Use absolute URLs with protocol
- Place in `<head>` section
- One per page
- Self-reference if no duplicates
- Update when URL structure changes

### Structured Data (JSON-LD)

#### What is Structured Data?
Structured data is a **standardized way to describe your content** so search engines understand it better.

**Think of it like this:**
- **Without structured data**: Search engine reads HTML and guesses what it means
- **With structured data**: You explicitly tell search engine "This is a product, here's the price, here's the rating"

#### Why Use It?
1. **Rich Results** - Enhanced search results (stars, prices, images)
2. **Better Understanding** - Search engines understand your content
3. **Voice Search** - Helps with voice assistants
4. **Knowledge Graph** - Can appear in Google's knowledge panel

#### JSON-LD Format
JSON-LD (JavaScript Object Notation for Linked Data) is the recommended format:
- Placed in `<head>` or `<body>`
- Doesn't affect page appearance
- Easy to maintain
- Can be dynamically generated

#### Basic Structure
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",  // Standard vocabulary
    "@type": "Product",                 // What type of thing is this?
    "name": "Laptop Pro 15",           // Properties of the thing
    "description": "High-performance laptop"
}
</script>
```

**Breaking it down:**
- `@context`: Tells search engine which vocabulary to use (schema.org)
- `@type`: What kind of thing this is (Product, Article, Organization, etc.)
- Properties: Details about the thing (name, price, description, etc.)

#### Example 1: Product (E-commerce)
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Product",
    "name": "Laptop Pro 15",
    "image": [
        "https://example.com/laptop-front.jpg",
        "https://example.com/laptop-back.jpg"
    ],
    "description": "High-performance laptop with 16GB RAM and 512GB SSD",
    "brand": {
        "@type": "Brand",
        "name": "TechBrand"
    },
    "offers": {
        "@type": "Offer",
        "url": "https://example.com/product/laptop",
        "priceCurrency": "USD",
        "price": "999.99",
        "priceValidUntil": "2024-12-31",
        "availability": "https://schema.org/InStock",
        "itemCondition": "https://schema.org/NewCondition"
    },
    "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.5",
        "reviewCount": "120",
        "bestRating": "5",
        "worstRating": "1"
    },
    "review": [
        {
            "@type": "Review",
            "reviewRating": {
                "@type": "Rating",
                "ratingValue": "5"
            },
            "author": {
                "@type": "Person",
                "name": "John Doe"
            },
            "reviewBody": "Great laptop, fast and reliable!"
        }
    ]
}
</script>
```

**Result in Google:** Shows price, rating stars, availability in search results

#### Example 2: Article (Blog Post)
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "How to Optimize Your Website for SEO",
    "description": "Complete guide to SEO optimization",
    "image": "https://example.com/seo-guide.jpg",
    "datePublished": "2024-01-15T10:00:00Z",
    "dateModified": "2024-01-20T14:30:00Z",
    "author": {
        "@type": "Person",
        "name": "Jane Smith",
        "url": "https://example.com/author/jane"
    },
    "publisher": {
        "@type": "Organization",
        "name": "TechBlog",
        "logo": {
            "@type": "ImageObject",
            "url": "https://example.com/logo.png"
        }
    },
    "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "https://example.com/blog/seo-guide"
    }
}
</script>
```

**Result in Google:** Shows article date, author, publisher logo in search results

#### Example 3: Organization (Company Info)
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "TechStore",
    "url": "https://example.com",
    "logo": "https://example.com/logo.png",
    "contactPoint": {
        "@type": "ContactPoint",
        "telephone": "+1-555-123-4567",
        "contactType": "customer service",
        "areaServed": "US",
        "availableLanguage": ["English", "Spanish"]
    },
    "sameAs": [
        "https://www.facebook.com/techstore",
        "https://www.twitter.com/techstore",
        "https://www.linkedin.com/company/techstore"
    ]
}
</script>
```

**Result in Google:** Can appear in knowledge panel with company info

#### Example 4: BreadcrumbList (Navigation)
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
        {
            "@type": "ListItem",
            "position": 1,
            "name": "Home",
            "item": "https://example.com"
        },
        {
            "@type": "ListItem",
            "position": 2,
            "name": "Products",
            "item": "https://example.com/products"
        },
        {
            "@type": "ListItem",
            "position": 3,
            "name": "Laptops",
            "item": "https://example.com/products/laptops"
        }
    ]
}
</script>
```

**Result in Google:** Shows breadcrumb navigation in search results

#### Example 5: FAQPage
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
        {
            "@type": "Question",
            "name": "What is the return policy?",
            "acceptedAnswer": {
                "@type": "Answer",
                "text": "We offer 30-day returns on all products in original condition."
            }
        },
        {
            "@type": "Question",
            "name": "Do you ship internationally?",
            "acceptedAnswer": {
                "@type": "Answer",
                "text": "Yes, we ship to over 50 countries worldwide."
            }
        }
    ]
}
</script>
```

**Result in Google:** Shows FAQ as expandable results in search

#### Example 6: LocalBusiness
```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "name": "TechStore Downtown",
    "image": "https://example.com/store-photo.jpg",
    "address": {
        "@type": "PostalAddress",
        "streetAddress": "123 Main St",
        "addressLocality": "New York",
        "addressRegion": "NY",
        "postalCode": "10001",
        "addressCountry": "US"
    },
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": 40.7128,
        "longitude": -74.0060
    },
    "url": "https://example.com",
    "telephone": "+1-555-123-4567",
    "openingHoursSpecification": [
        {
            "@type": "OpeningHoursSpecification",
            "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
            "opens": "09:00",
            "closes": "18:00"
        }
    ],
    "priceRange": "$$"
}
</script>
```

**Result in Google:** Shows business info, map, hours in local search

#### JavaScript Example (Dynamic)
```javascript
// Generate structured data dynamically
function generateProductSchema(product) {
    return {
        "@context": "https://schema.org",
        "@type": "Product",
        "name": product.name,
        "image": product.images,
        "description": product.description,
        "brand": {
            "@type": "Brand",
            "name": product.brand
        },
        "offers": {
            "@type": "Offer",
            "priceCurrency": product.currency,
            "price": product.price.toString(),
            "availability": product.inStock 
                ? "https://schema.org/InStock" 
                : "https://schema.org/OutOfStock"
        }
    };
}

// Add to page
function addStructuredData(data) {
    const script = document.createElement('script');
    script.type = 'application/ld+json';
    script.text = JSON.stringify(data);
    document.head.appendChild(script);
}

// Usage
const productSchema = generateProductSchema({
    name: "Laptop Pro 15",
    images: ["https://example.com/laptop.jpg"],
    description: "High-performance laptop",
    brand: "TechBrand",
    currency: "USD",
    price: 999.99,
    inStock: true
});

addStructuredData(productSchema);
```

#### Common Schema Types
- **Product** - E-commerce products
- **Article** - Blog posts, news articles
- **Organization** - Company information
- **LocalBusiness** - Physical business locations
- **BreadcrumbList** - Site navigation
- **FAQPage** - Frequently asked questions
- **Review** - Product/service reviews
- **Recipe** - Cooking recipes
- **Event** - Events, concerts, conferences
- **VideoObject** - Videos
- **Person** - Person profiles

#### Testing Your Structured Data
1. **Google Rich Results Test**: https://search.google.com/test/rich-results
2. **Schema.org Validator**: https://validator.schema.org/
3. **Google Search Console** - Check for errors

#### Best Practices
- Use JSON-LD format (recommended by Google)
- Place in `<head>` section
- One structured data per page (can have multiple types)
- Match structured data to actual page content
- Keep it accurate and up-to-date
- Test with Google's Rich Results Test
- Don't add structured data for content that doesn't exist

### Heading Structure
```html
<h1>Main Page Title</h1>
    <h2>Section Title</h2>
        <h3>Subsection</h3>
    <h2>Another Section</h2>
```
- One H1 per page
- Logical hierarchy (H1 → H2 → H3)
- Include keywords naturally
- Don't skip levels

### URL Structure
```
✅ Good:
https://example.com/products/laptops/gaming
https://example.com/blog/seo-guide-2024

❌ Bad:
https://example.com/page?id=123
https://example.com/products?cat=1&sub=5
```
- Use descriptive, readable URLs
- Include keywords
- Use hyphens, not underscores
- Keep URLs short
- Use HTTPS

### Internal Linking
- Link to important pages
- Use descriptive anchor text
- Create logical site structure
- Use breadcrumbs
- Link related content

### Image Optimization
```html
<img src="laptop.jpg" 
     alt="Gaming laptop with RGB keyboard" 
     width="800" 
     height="600"
     loading="lazy">
```
- **Alt text**: Descriptive, include keywords
- **File names**: Descriptive (laptop-gaming.jpg)
- **Size**: Optimize file size
- **Format**: WebP, AVIF when possible
- **Lazy loading**: For below-fold images

## Content SEO

### Keyword Research
- Use tools: Google Keyword Planner, Ahrefs, SEMrush
- Target long-tail keywords
- Analyze competitor keywords
- Consider search intent

### Content Quality
- Original, valuable content
- Answer user questions
- Comprehensive coverage
- Regular updates
- Proper length (300+ words minimum)

### Keyword Placement
- In title tag
- In H1 heading
- First paragraph
- Throughout content naturally
- In image alt text
- In URL

## Performance SEO

### Page Speed
- **Core Web Vitals**:
  - LCP (Largest Contentful Paint) < 2.5s
  - FID (First Input Delay) < 100ms
  - CLS (Cumulative Layout Shift) < 0.1

### Optimization Techniques
```javascript
// Code splitting
const LazyComponent = lazy(() => import('./Component'));

// Image optimization
import Image from 'next/image';
<Image src="/image.jpg" width={800} height={600} />

// Preload critical resources
<link rel="preload" href="/font.woff2" as="font" type="font/woff2" crossorigin>

// Defer non-critical CSS
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### Mobile Optimization
```html
<!-- Responsive viewport -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Mobile-friendly test -->
<!-- Use Google's Mobile-Friendly Test -->
```
- Responsive design
- Touch-friendly buttons
- Fast mobile load times
- No horizontal scrolling

## Off-Page SEO

### Backlinks
- Quality over quantity
- Natural link building
- Relevant sources
- Diverse anchor text

### Social Signals
- Social sharing
- Social engagement
- Brand mentions

## Tools & Resources

### Google Tools
- **Google Search Console** - Monitor performance
- **Google Analytics** - Traffic analysis
- **PageSpeed Insights** - Performance testing
- **Mobile-Friendly Test** - Mobile optimization
- **Rich Results Test** - Structured data validation

### Other Tools
- **Ahrefs** - Keyword research, backlinks
- **SEMrush** - SEO analysis
- **Screaming Frog** - Technical SEO audit
- **Schema.org Validator** - Structured data

## Best Practices Checklist

### Technical
- [ ] SSR/SSG for SEO-critical pages
- [ ] Proper robots.txt
- [ ] Sitemap.xml submitted
- [ ] HTTPS enabled
- [ ] Fast page load (< 3 seconds)
- [ ] Mobile-friendly
- [ ] No broken links
- [ ] Proper redirects (301 for permanent)

### On-Page
- [ ] Unique title tags (50-60 chars)
- [ ] Meta descriptions (150-160 chars)
- [ ] One H1 per page
- [ ] Proper heading hierarchy
- [ ] Canonical URLs set
- [ ] Structured data (JSON-LD)
- [ ] Image alt text
- [ ] Internal linking

### Content
- [ ] Keyword research done
- [ ] Quality, original content
- [ ] Regular content updates
- [ ] User-focused content
- [ ] Proper keyword density (1-2%)

## Common Mistakes
- Keyword stuffing
- Duplicate content
- Thin content
- Slow page speed
- Not mobile-friendly
- Broken links
- Missing alt text
- Poor internal linking
- Ignoring technical SEO