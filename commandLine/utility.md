free space node_modules

`linux/mac
## view space that can be freed :
`find . -name "node_modules" -type d -prune | xargs du -chs

## delete
`find . -name "node_modules" -type d -prune -exec rm -rf '{}' +