# AEO and SEO Discovery

### Feature ID:

**[IAA-SEO-003]**

### Title:

**AEO and SEO Discovery for Merchants**

### Category:

**Shop Builder | Marketing & Discovery** | **Actors**: Merchant, AI Engine, Search Engines | **Channel**: Web Dashboard, Shopfront, API

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Search behavior is rapidly shifting toward AI answer engines (ChatGPT, Perplexity, Gemini), but merchant products are not optimized for these systems. Traditional SEO alone is insufficient as AI engines prioritize structured data, semantic understanding, and direct answer formats. This results in missed AI-driven organic traffic and lack of differentiation for Droplinked merchants.

**Desired Outcome:**

- Every product page includes complete JSON-LD structured data
- AI-optimized product description generation
- AEO scoring dashboard for merchants
- AI search performance analytics
- AEO implementation requires zero technical knowledge from merchants

---

### 2) **Scope – In / Out**

**In Scope:**

- **Automatic Schema Generation**
    - Automatic JSON-LD schema generation for all products
    - Schema types: Product, FAQPage, HowTo, Organization, WebSite
    - Structured data for FAQ, How-To, and Product schemas
    - Semantic markup for brand values and product attributes
- **AI Content Optimization**
    - AI-optimized product description generation
    - AI engine generates 3 AI-optimized title variants per product
    - AI-friendly product description (150-200 words)
    - Image SEO with AI-generated alt text
- **Dashboard & Analytics**
    - AEO scoring dashboard for merchants
    - Product-Level AEO Scores (0-100)
    - Optimization suggestions
    - AI search performance analytics
    - Analytics track AI search impressions by source (ChatGPT, Perplexity, Gemini)
- **Shopfront Enhancements**
    - Integration with major AI search engines
    - Automatic metadata optimization
    - Rich snippet preparation
    - Voice search optimization hints
    - Knowledge panel optimization for brands

**Out of Scope:**

- Manual schema markup editing (merchant-facing)
- Third-party SEO tool integrations (Ahrefs, SEMrush)
- Paid search advertising features
- Social media optimization
- Email marketing SEO
- Content marketing blog SEO (separate feature)
- Technical SEO audits (site speed, mobile responsiveness)
- Backlink building tools
- Competitor SEO analysis

---

### 3) **Key User Journeys**

---

**Journey 1: Automatic AEO Setup**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Creates Product | AI Engine Analyzes Product Data |
| 2 | System | Generates Structured Schema Markup (JSON-LD) | Schema injected into product page |
| 3 | System | Creates Semantic Brand Values | Metadata enriched |
| 4 | System | Optimizes Product Descriptions for AI | AI-optimized content generated |
| 5 | System | Auto-Publishes to Shopfront | Enhanced content live |
| 6 | System | AI Search Engines Index Enhanced Content | Product discoverable by AI engines |

---

**Journey 2: AEO Dashboard Review**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to SEO/AEO Dashboard | Dashboard loads with product scores |
| 2 | Merchant | Views Product-Level AEO Scores | Score (0-100) displayed per product |
| 3 | Merchant | Sees Optimization Suggestions | Specific improvements listed |
| 4 | Merchant | Applies AI-Suggested Improvements | Suggestions applied |
| 5 | System | Scores Update in Real-Time | Updated score displayed |

---

**Journey 3: AI Search Performance Monitoring**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens AEO Analytics | Analytics dashboard loads |
| 2 | Merchant | Views AI Search Impressions | Data from ChatGPT, Perplexity, Gemini shown |
| 3 | Merchant | Tracks Answer Engine Referral Traffic | Traffic sources displayed |
| 4 | Merchant | Compares Traditional SEO vs AEO Performance | Comparison chart shown |
| 5 | Merchant | Identifies Top Performing Products in AI Search | Top products listed |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Schema & Structured Data** |  |
| BAC-1 | Every product page includes complete JSON-LD structured data |
| BAC-2 | Schema types include: Product, FAQPage, HowTo, Organization, WebSite |
| BAC-3 | FAQ schema automatically created from product Q&A |
| BAC-4 | Semantic markup for brand values and product attributes |
| **AI Content Generation** |  |
| BAC-5 | AI engine generates 3 AI-optimized title variants per product |
| BAC-6 | AI engine generates AI-friendly product description (150-200 words) |
| BAC-7 | Image alt text auto-generated using AI vision analysis |
| BAC-8 | Knowledge panel data structured for brand recognition |
| **Dashboard & Analytics** |  |
| BAC-9 | AEO dashboard displays score (0-100) for each product |
| BAC-10 | Dashboard shows specific optimization suggestions |
| BAC-11 | Analytics track AI search impressions by source (ChatGPT, Perplexity, Gemini) |
| BAC-12 | Merchants can view AI search referral traffic in dashboard |
| BAC-13 | AEO implementation requires zero technical knowledge from merchants |
| BAC-14 | Page load time impact < 100ms from schema injection |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-003 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **AEO** | Answer Engine Optimization - optimizing content for AI answer engines |
| **SEO** | Search Engine Optimization - traditional search engine ranking |
| **JSON-LD** | JavaScript Object Notation for Linked Data - structured data format |
| [**Schema.org**](http://schema.org/) | Vocabulary for structured data markup |
| **Knowledge Panel** | Information box displayed by search engines about entities |
| **Semantic Markup** | HTML tags that provide meaning to content |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Schema Generation Logic**

```
ON ProductCreate OR ProductUpdate:
    product = getProductData(product_id)

    // Generate Product Schema
    product_schema = {
        "@context": "<https://schema.org>",
        "@type": "Product",
        "name": product.name,
        "description": product.description,
        "image": product.images,
        "sku": product.sku,
        "brand": {
            "@type": "Brand",
            "name": product.brand_name
        },
        "offers": {
            "@type": "Offer",
            "price": product.price,
            "priceCurrency": product.currency,
            "availability": product.stock_status,
            "url": product.url
        },
        "aggregateRating": {
            "@type": "AggregateRating",
            "ratingValue": product.avg_rating,
            "reviewCount": product.review_count
        }
    }

    // Generate FAQ Schema if Q&A exists
    IF product.faq_count > 0:
        faqs = getProductFAQs(product_id)
        faq_schema = {
            "@context": "<https://schema.org>",
            "@type": "FAQPage",
            "mainEntity": MAP faqs TO {
                "@type": "Question",
                "name": faq.question,
                "acceptedAnswer": {
                    "@type": "Answer",
                    "text": faq.answer
                }
            }
        }

    // Generate Organization Schema
    org_schema = {
        "@context": "<https://schema.org>",
        "@type": "Organization",
        "name": shop.name,
        "url": shop.url,
        "logo": shop.logo_url,
        "sameAs": shop.social_profiles
    }

    // Store schemas for injection
    STORE product_schema, faq_schema, org_schema IN product.seo_data
```

---

### **FL-2: AI Content Optimization**

```
FUNCTION generateAIOptimizedContent(product):

    // Generate title variants
    title_variants = AI.generate(
        prompt: "Generate 3 SEO-optimized product titles",
        context: {
            product_name: product.name,
            category: product.category,
            key_features: product.features
        },
        constraints: {
            max_length: 60,
            include_keywords: true
        }
    )

    // Generate AI-friendly description
    ai_description = AI.generate(
        prompt: "Generate AI-optimized product description",
        context: {
            product: product,
            target_length: "150-200 words"
        },
        style: "conversational, informative, answer-focused"
    )

    // Generate image alt texts
    FOR each image IN product.images:
        image.alt_text = AI.vision_analyze(
            image: image.url,
            task: "generate_descriptive_alt_text"
        )

    RETURN {
        title_variants: title_variants,
        ai_description: ai_description,
        image_alts: image_alt_texts
    }
```

---

### **FL-3: AEO Score Calculation**

```
FUNCTION calculateAEOScore(product):
    score = 0
    max_score = 100

    // Schema completeness (30 points)
    IF has_product_schema: score += 10
    IF has_faq_schema: score += 10
    IF has_org_schema: score += 10

    // Content optimization (40 points)
    IF ai_description_exists: score += 15
    IF ai_description_quality >= 0.8: score += 10
    IF all_images_have_alt: score += 10
    IF title_optimized: score += 5

    // Technical factors (30 points)
    IF page_load_time < 2s: score += 10
    IF mobile_optimized: score += 10
    IF has_canonical_url: score += 5
    IF has_meta_description: score += 5

    // Penalties
    IF broken_links: score -= 10
    IF duplicate_content: score -= 15

    RETURN MIN(MAX(score, 0), max_score)

ON ScoreRequest(product_id):
    product = getProduct(product_id)
    score = calculateAEOScore(product)

    // Generate suggestions
    suggestions = []
    IF NOT product.ai_description:
        suggestions.push("Generate AI-optimized description")
    IF NOT product.faq_count:
        suggestions.push("Add product FAQ section")
    IF NOT all_images_have_alt:
        suggestions.push("Add alt text to all images")

    RETURN {
        score: score,
        suggestions: suggestions,
        last_updated: NOW
    }
```

---

### **FL-4: Analytics Tracking**

```
ON PageLoad(product_id):
    // Track AI search engine impressions
    IF referrer IN ['chat.openai.com', 'perplexity.ai', 'gemini.google.com']:
        LOG event: 'ai_search_impression',
            source: extractSource(referrer),
            product_id: product_id,
            timestamp: NOW

    // Track traditional SEO
    IF referrer IN ['google.com', 'bing.com']:
        LOG event: 'search_impression',
            source: extractSource(referrer),
            product_id: product_id

FUNCTION getAEOAnalytics(merchant_id, date_range):
    RETURN {
        ai_impressions: {
            chatgpt: COUNT events WHERE source='chatgpt',
            perplexity: COUNT events WHERE source='perplexity',
            gemini: COUNT events WHERE source='gemini'
        },
        traditional_impressions: {
            google: COUNT events WHERE source='google',
            bing: COUNT events WHERE source='bing'
        },
        top_products: GET top 10 products by impression count,
        trend: GET impressions over time
    }
```

---

### B3) Behavioral Flow

```
[Merchant Creates/Updates Product]
    ↓
[Trigger AEO Processing]
    ↓
[Generate JSON-LD Schemas]
    ├─ Product Schema
    ├─ FAQ Schema (if applicable)
    └─ Organization Schema
    ↓
[AI Content Generation]
    ├─ Title Variants
    ├─ AI Description
    └─ Image Alt Texts
    ↓
[Calculate AEO Score]
    ↓
[Store All Data]
    ↓
[Inject Schemas into Shopfront]
    ↓
[AI Engines Index Content]
    ↓
[Analytics Track Performance]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| No Schema | Product created | Schema Generated | JSON-LD created and stored |
| Schema Generated | Content updated | Re-optimized | AI content regenerated |
| Optimized | Score calculated | Scored | Score displayed in dashboard |
| Scored | Improvement applied | Improved | Score recalculated |
| Indexed | AI engine crawls | Discoverable | Traffic potential increased |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** AI generation fails | Use original content, flag for retry |
| **EC-2:** Schema validation fails | Log error, use fallback schema |
| **EC-3:** Product has no images | Skip alt text generation |
| **EC-4:** FAQ empty | Don't generate FAQ schema |
| **EC-5:** AI service unavailable | Queue for retry, use cached content |
| **EC-6:** Merchant rejects AI suggestions | Store preference, don't regenerate |
| **EC-7:** Duplicate product detected | Flag for canonical URL |

---

### B6) Data Requirements

**SEO Data Storage:**

| Field | Type | Description |
| --- | --- | --- |
| product_id | UUID | Reference to product |
| json_ld_schemas | JSON | Structured data schemas |
| ai_title_variants | Array | AI-generated title options |
| ai_description | Text | AI-optimized description |
| aeo_score | Integer | Current AEO score (0-100) |
| score_last_calculated | DateTime | When score was last updated |
| optimization_suggestions | JSON | List of improvement suggestions |

**Analytics Events:**

| Event Type | Data Tracked | Storage |
| --- | --- | --- |
| ai_search_impression | source, product_id, timestamp | Analytics DB |
| search_impression | source, product_id, timestamp | Analytics DB |
| aeo_score_change | old_score, new_score, product_id | Audit Log |
| schema_generated | schema_type, product_id | System Log |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Create new product | JSON-LD schema generated |
| TC-2 | View AEO dashboard | Score displayed (0-100) |
| TC-3 | Check schema injection | Schema present in page HTML |
| TC-4 | AI content generation | 3 title variants created |
| TC-5 | Apply optimization suggestion | Score increases |
| TC-6 | View analytics | AI impressions tracked by source |
| TC-7 | Mobile page load | Schema injection < 100ms |
| TC-8 | FAQ schema generation | FAQ schema created from Q&A |
| TC-9 | Image alt generation | Alt texts auto-generated |
| TC-10 | Score recalculation | Score updates after improvements |