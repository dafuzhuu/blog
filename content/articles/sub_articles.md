+++
title = 'Hugo设置文章隐藏功能'
date = 2024-08-31T10:38:24+08:00
tags = ['Hugo']
draft = false
+++

文章分为两种，一种为主干，另一种是分支。好的博文会将主干写的清晰简洁，而技术细节会放在分支里。在Hugo中，无论是主干还是分支都会显示在Archive中，这是我们不希望看见的。我们希望隐藏分支，在Archive中只看到主干文章，但仍能通过url访问到分支。

Hugo主文件夹中的 `layouts` 可以覆盖theme中的配置，因此可以将主题中 `layouts/_default/archives.html` 复制到主文件夹的 `layouts` 下，并进行修改。

以PaperMod主题为例

```html
<div class="archive-posts">
      {{- range .Pages }}
      {{- if not .Params.hidden }}  <!-- 添加hidden逻辑 -->
      {{- if eq .Kind "page" }}
      <div class="archive-entry">
        <h3 class="archive-entry-title entry-hint-parent">
          {{- .Title | markdownify }}
          {{- if .Draft }}
          <span class="entry-hint" title="Draft">
            <svg xmlns="http://www.w3.org/2000/svg" height="15" viewBox="0 -960 960 960" fill="currentColor">
              <path
                d="M160-410v-60h300v60H160Zm0-165v-60h470v60H160Zm0-165v-60h470v60H160Zm360 580v-123l221-220q9-9 20-13t22-4q12 0 23 4.5t20 13.5l37 37q9 9 13 20t4 22q0 11-4.5 22.5T862.09-380L643-160H520Zm300-263-37-37 37 37ZM580-220h38l121-122-18-19-19-18-122 121v38Zm141-141-19-18 37 37-18-19Z" />
          </svg>
          </span>
          {{- end }}
        </h3>
        <div class="archive-meta">
          {{- partial "post_meta.html" . -}}
        </div>
        <a class="entry-link" aria-label="post link to {{ .Title | plainify }}" href="{{ .Permalink }}"></a>
      </div>
      {{- end }}
      {{- end }}
      {{- end }}  <!-- 隐藏 -->
    </div>
  </div>
```

Hugo对于非默认的章节参数都会存储在 `.Params` 下面，通过

```html
{{- if not .Params.hidden }}
    ...
{{- end }}
```

可以隐藏 `hidden = true` 的文章，使其不显示在Archive中

```toml
+++
title = '隐藏文章'
date = 2024-08-31T10:38:24+08:00
draft = false
hidden = true
+++
```
