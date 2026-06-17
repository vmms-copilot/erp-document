# Website Module (Website / Storefront CMS) — Deep Documentation

Module path: `/website/*` · Menu: "Website" · businessId=137541
A full storefront website-builder/CMS. The connected store publishes to a Nhanh-hosted site (e.g. `phuonganhnguyena9.store.nhanhapp.com`) or a custom domain.

Menu routes (captured from nav DOM):
- Nội dung (Content / template variables) → `/website/content/index`
- Cài đặt (Settings) → `/website/content/setting`
- Menu → `/website/menu/index`
- Tin tức (News/blog) → `/website/article/index`
- Tag → `/website/content/tags`
- Banner → `/website/banner/index`
- Album → `/website/album/index`
- Liên hệ (Contact submissions) → `/website/contact/index`
- Newsletter → `/website/newsletter/index`
- Javascript (custom JS injection) → `/website/script/index`
- Link điều hướng (Redirects) → `/website/redirectlink/index`
- Ngôn ngữ (Translations/i18n) → `/website/translate/index`
- User đăng nhập (Storefront login users) → `/website/user/index`
- Bình luận, đánh giá (Comments/reviews) → submenu

---

## 1. Content / Template Variables — `/website/content/index`

A key-value template-variable editor that drives the storefront theme. Template-scope tabs: **Trang chủ** (Homepage) · **Chân trang** (Footer) · **Trang chi tiết sản phẩm** (Product detail) · **Trang liên hệ** (Contact page). A domain selector + **Xem website** (preview) + **Xoá cache** (clear cache).

### Columns: Key name | Giá trị mặc định (default) | Giá trị (override)

### Sample keys (storefront config surface)
HOT_LINE, TITLE_WEB, GMAIL, ADDRESS_STORE, SOCIAL_FACEBOOK/YOUTUBE/PINTERREST/MAPS, STORE_INTRODUCE, INSTA_LINK/INSTA_TITLE, **META_TITLE / META_DESCRIPTION / META_KEYWORDS** (SEO), PRICE_CONTACT, ORDER_SUCCESSFUL_TITLE, ORDER_SUCCESS_CART(_CONT), STORE_TITLE_HEADERR, DETAIL_TITLE_PRODUCT, TEXT_BEST/NEW/HOT_PRODUCT, TITLE_BESTSELLER, HOTLINE_WHOLESALE/RETAIL, SEARCH_SUGGEST, MOMO/MOMO_CHECKOUT (payment gateway text), PAYMENT_ONLINE_CUSTOMER, CONTENT_HOME_ABOUT, TITLE_WHY_CHOOSE, TITLE_NEWS_HOME, BCT_LINK (Bộ Công Thương registration), CHECKBOX_POLICY.

Compliance note on page: register the website with Bộ Công Thương per Nghị định 85/2021 & 98/2020 (fine up to 30M VND otherwise).

---

## 2. News / Blog — `/website/article/index`

Tabs: Danh sách tin tức (Articles) / Danh mục tin tức (Categories). 3 articles present.
Filters: ID, Tiêu đề, Danh mục tin tức, Ngày tạo, Trạng thái.
Columns: ID, Danh mục (e.g. THỜI TRANG 2022), Ảnh, Tiêu đề, **Lượt xem** (views), Ngày tạo / Ngày đăng (published) / Ngày hạ (unpublish), Thứ tự (sort order), active, Người tạo/sửa, actions. Thêm mới opens a rich-text article editor.

---

## 3. Other CMS pages (routes confirmed)
- **Menu** `/website/menu/index` — storefront navigation menu builder
- **Banner** `/website/banner/index` — homepage banners/sliders
- **Album** `/website/album/index` — image galleries
- **Tag** `/website/content/tags` — content tags
- **Liên hệ** `/website/contact/index` — contact-form submissions inbox
- **Newsletter** `/website/newsletter/index` — email subscriber list
- **Javascript** `/website/script/index` — inject custom JS (analytics/pixels)
- **Link điều hướng** `/website/redirectlink/index` — 301/302 redirect rules (SEO)
- **Ngôn ngữ** `/website/translate/index` — i18n string overrides
- **User đăng nhập** `/website/user/index` — storefront customer accounts
- **Cài đặt** `/website/content/setting` — website settings (domain, theme, payment, SEO)

---

## API / route summary (Website)
- `GET /website/content/index` — template variables (homepage/footer/product/contact)
- `GET /website/article/index` — news/blog (articles + categories)
- `GET /website/menu|banner|album|contact|newsletter|script|redirectlink|translate|user/index`

## Entities surfaced
- **WebsiteConfig (template var)**: key, defaultValue, value, scope(home/footer/productDetail/contact), domain
- **Article**: id, categoryId, title, image, body(html), views, createdAt, publishedAt, unpublishAt, sortOrder, active, author
- **ArticleCategory**: id, name
- **Menu**: id, items[]{label,url,parent,order}
- **Banner**: id, image, link, position, order, active
- **ContactSubmission**: id, name, email, phone, message, createdAt
- **NewsletterSubscriber**: email, subscribedAt
- **RedirectLink**: from, to, type(301/302)
- **StorefrontUser**: id, name, email, phone, registeredAt
