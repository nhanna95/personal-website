---
layout: default
title: Blog
permalink: /blog.html
---

<div class="blog-head">
    <h1 class="h1">Blog</h1>
    <div class="search-wrap">
        <form class="searchbar" role="search" aria-label="Search blog posts" onsubmit="return false;">
            <span class="icon">🔍</span>
            <input id="q" type="search" placeholder="Search posts" autocomplete="off" />
        </form>
    </div>
</div>

<!-- Tag Filter Bar (UNION) -->
<form id="filters" class="tag-filter" aria-label="Filter posts by tag">
    <!-- Tags will be injected dynamically -->
    <button type="reset" class="tag reset">Reset</button>
</form>

<section class="post-list" id="welcome-post"></section>
<section class="post-list" id="post-list">
    <!-- Posts will be injected from Jekyll collections -->
</section>

<script>
    (function () {
        const toggle = document.getElementById('menu-toggle');
        const sidebar = document.querySelector('.sidebar');
        if (!toggle || !sidebar) return;
        toggle.addEventListener('click', function () {
            const open = sidebar.classList.toggle('is-menu-open');
            toggle.setAttribute('aria-expanded', String(open));
            document.body.style.overflow = open ? 'hidden' : '';
        });
    })();

    // Simple client-side search on blog list
    (function () {
        const q = document.getElementById('q');
        const list = document.getElementById('post-list');
        if (!q || !list) return;
        function filter() {
            const term = q.value.trim().toLowerCase();
            const rows = [...list.querySelectorAll('.post')];
            rows.forEach(r => {
                const hay = (r.querySelector('.post-title').textContent + ' ' + (r.getAttribute('data-tags') || '')).toLowerCase();
                r.style.display = term === '' || hay.includes(term) ? 'grid' : 'none';
            });
        }
        q.addEventListener('input', filter);
    })();

    (function () {
        // Get posts from Jekyll-generated data
        const posts = [
            {% for post in site.posts %}
            {
                title: {{ post.title | jsonify }},
                url: "{{ post.url | relative_url }}",
                date: "{{ post.date | date: '%Y-%m-%d' }}",
                tags: {{ post.tags | jsonify }}
            }{% unless forloop.last %},{% endunless %}
            {% endfor %}
        ];
        
        const list = document.getElementById('post-list');
        const welcome = document.getElementById('welcome-post');
        const filters = document.getElementById('filters');
        
        if (!list || !filters) return;

        // Aggregate tag counts (exclude empty strings)
        const tagToCount = new Map();
        posts.forEach(p => (p.tags || []).forEach(t => {
            const k = String(t).trim();
            if (!k) return;
            tagToCount.set(k, (tagToCount.get(k) || 0) + 1);
        }));

        // Render tag checkboxes sorted by desc count, then alpha
        const sortedTags = [...tagToCount.entries()]
            .sort((a, b) => b[1] - a[1] || a[0].localeCompare(b[0]))
            .map(([name, count]) => ({ name, count }));

        const resetBtn = filters.querySelector('.reset');
        if (resetBtn) filters.removeChild(resetBtn);
        sortedTags.forEach(({ name, count }) => {
            const id = `tag-${name.toLowerCase().replace(/[^a-z0-9]+/g, '-')}`;
            const input = document.createElement('input');
            input.type = 'checkbox';
            input.id = id;
            input.value = name.toLowerCase();
            const label = document.createElement('label');
            label.htmlFor = id;
            label.className = 'tag';
            label.textContent = `${name} (${count})`;
            filters.appendChild(input);
            filters.appendChild(label);
        });
        // Re-add reset at end and wire up behavior
        const reset = document.createElement('button');
        reset.type = 'reset';
        reset.className = 'tag reset';
        reset.textContent = 'Reset';
        filters.appendChild(reset);

        // Pull out the welcome post and render it separately
        const welcomePost = posts.find(p => p.url.includes('welcome'));
        const regularPosts = posts.filter(p => !p.url.includes('welcome'));

        if (welcomePost) {
            const h2 = document.createElement('h2');
            h2.className = 'year';
            h2.textContent = 'About My Blog';
            welcome.appendChild(h2);

            const art = document.createElement('article');
            art.className = 'post';
            const wfmt = new Intl.DateTimeFormat(undefined, { month: 'short', day: 'numeric' });
            const wdateHTML = `<time class="post-date" datetime="${welcomePost.date}">${wfmt.format(new Date(welcomePost.date))}</time>`;
            art.innerHTML = `
                <div class="post-head"><a class="post-title" href="${welcomePost.url}">${welcomePost.title}</a><span class="post-readtime" aria-label="Estimated read time"></span></div>
                ${wdateHTML}
            `;
            welcome.appendChild(art);

            // Compute read time for welcome post
            (async () => {
                try {
                    const r = await fetch(welcomePost.url);
                    if (!r.ok) throw new Error('failed');
                    const html = await r.text();
                    const tmp = document.createElement('div');
                    tmp.innerHTML = html;
                    const entry = tmp.querySelector('article.entry') || tmp;
                    const text = (entry.textContent || '').trim();
                    const words = text.split(/\s+/).filter(Boolean).length;
                    const minutes = Math.max(1, Math.ceil(words / 225));
                    const rt = art.querySelector('.post-readtime');
                    if (rt) rt.textContent = `· ${minutes} min read`;
                } catch (e) {
                    // ignore failures
                }
            })();
        }

        let currentYear = null;
        const fmt = new Intl.DateTimeFormat(undefined, { month: 'short', day: 'numeric' });
        regularPosts.forEach(p => {
            const d = new Date(p.date);
            const year = d.getFullYear();
            if (year !== currentYear) {
                currentYear = year;
                const h2 = document.createElement('h2');
                h2.className = 'year';
                h2.textContent = String(year);
                list.appendChild(h2);
            }
            const art = document.createElement('article');
            art.className = 'post';
            art.setAttribute('data-tags', (p.tags || []).map(t => t.toLowerCase()).join(' '));
            const dateHTML = `<time class="post-date" datetime="${p.date}">${fmt.format(d)}</time>`;
            const readHTML = `<span class="post-readtime" aria-label="Estimated read time"></span>`;
            art.innerHTML = `
                <div class="post-head"><a class="post-title" href="${p.url}">${p.title}</a>${readHTML}</div>
                ${dateHTML}
                ${p.tags && p.tags.length ? `<ul class="taglist">${p.tags.map(t => `<li>${t}</li>`).join('')}</ul>` : ''}
            `;
            list.appendChild(art);

            // Compute read time asynchronously
            (async () => {
                try {
                    const r = await fetch(p.url);
                    if (!r.ok) throw new Error('failed');
                    const html = await r.text();
                    const tmp = document.createElement('div');
                    tmp.innerHTML = html;
                    const entry = tmp.querySelector('article.entry') || tmp;
                    const text = (entry.textContent || '').trim();
                    const words = text.split(/\s+/).filter(Boolean).length;
                    const minutes = Math.max(1, Math.ceil(words / 225));
                    const rt = art.querySelector('.post-readtime');
                    if (rt) rt.textContent = `· ${minutes} min read`;
                } catch (e) {
                    // ignore failures
                }
            })();
        });

        function applyFilter() {
            const selected = [...filters.querySelectorAll('input[type="checkbox"]:checked')]
                .map(i => i.value);
            const rows = [...list.querySelectorAll('.post')];
            if (selected.length === 0) {
                rows.forEach(r => r.style.display = 'grid');
                return;
            }
            rows.forEach(r => {
                const tags = (r.getAttribute('data-tags') || '').split(/\s+/);
                const show = selected.some(s => tags.includes(s));
                r.style.display = show ? 'grid' : 'none';
            });
        }

        filters.addEventListener('change', applyFilter);
        filters.addEventListener('reset', function () {
            setTimeout(applyFilter, 0);
        });
        applyFilter();
    })();
</script>

