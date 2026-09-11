# Sites and domains

Site updates expose non-sensitive presentation, SEO, language, delivery and
domain settings only. Git, CDN, search-engine and deployment credentials are
configured through Console pages. A site has one GitBook UI language. Custom
domain creation uses `domain_name` and defaults `scope` to `global`. Creating a
new domain replaces the site's previous CDN configuration. It does not change
DNS; the CNAME check requires confirmation because a successful check starts
CDN configuration and may update the site's canonical URL.
