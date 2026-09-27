# Jon Rogol — personal website

Live at **[acloudyday.net](https://acloudyday.net/)**. A personal portfolio focused on network engineering, enterprise and healthcare environments, and a practical AWS hosting project.

## Current website

The production website lives in [`Website/`](Website/). It uses semantic HTML and responsive CSS; there is no build step or client-side JavaScript dependency. Portrait images use responsive WebP sources with a JPEG fallback. The site includes keyboard focus styles, a skip link, reduced-motion support, print styles, structured profile data, and a social sharing image.

```text
Browser → HTTPS → Amazon CloudFront → Amazon S3
```

GitHub holds the source and change history. `Example Website/` and `HTML Templates/` contain earlier reference material; they are not the current deployed frontend. Their sample backend code is not a claim that a visitor counter or API is running on the live site.

## Preview locally

From the repository root:

```sh
python3 -m http.server 8765 --bind 127.0.0.1 --directory Website
```

Open `http://127.0.0.1:8765/`. Check narrow mobile and desktop widths, keyboard navigation, internal anchors, external links, and image loading before publishing changes.

## Publish

The existing AWS deployment uses the `acloudyday.net` S3 bucket and CloudFront distribution `E194NF418BRUEG`. Use an authorized AWS session. Preview changed files before uploading; do not delete other bucket contents.

```sh
aws s3 sync Website/ s3://acloudyday.net/ --cache-control 'public,max-age=300' --dryrun
aws s3 sync Website/ s3://acloudyday.net/ --cache-control 'public,max-age=300'
aws cloudfront create-invalidation --distribution-id E194NF418BRUEG --paths '/*'
```

Verify the invalidation completes and the public page and changed assets match the release. When editing the stylesheet, update its version query in `index.html` so returning visitors request the latest CSS.
