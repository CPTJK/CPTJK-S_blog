# vite build gzip
## 第一步, 安装需要的插件
`vite-plugin-compression`[github链接](https://github.com/vbenjs/vite-plugin-compression/tree/main#readme)
> 点击链接可以看到比较详细的用法介绍
```shell
npm install vite-plugin-compression -D
```
## 第二步, 配置vite.config.js(ts)
```javascript
import viteCompression from 'vite-plugin-compression';

export default () => {
  return {
    plugins: [viteCompression()],
  };
};
```
**关于viteCompression的配置介绍:**

| params | type | default | desc |
| ------ | ---- | ------- | ---- |
| verbose | `boolean` | `true` | Whether to output the compressed result in the console |
| filter | `RegExp or (file: string) => boolean` | `DefaultFilter` | Specify which resources are not compressed |
| disable | `boolean` | `false` | Whether to disable |
| threshold | `number` | `1025` | It will be compressed if the volume is larger than threshold, the unit is b |
| algorithm | `string` | `gzip` | Compression algorithm, optional ['gzip','brotliCompress' ,'deflate','deflateRaw'] |
| ext | `string` | `.gz` | Suffix of the generated compressed package |
| compressionOptions | `object` | - | The parameters of the corresponding compression algorithm |
| deleteOriginFile | `boolean` | - | Whether to delete source files after compression |

**DefaultFilter**

`/\.(js|mjs|json|css|html)$/i`

## 第三步, 配置Nginx

经过上面的配置后, 运行 `npm run build` 命令, 打包的产物就会有`.gz`文件. <br> 
现代浏览器是支持gzip压缩文件的, 但是你需要在Nginx进行相关配置以支持响应gzip文件

```shell
# 启用 gzip 压缩
gzip on;
# 设置压缩的静态文件的压缩比率，优先级低于 gzip_comp_level
gzip_static on;
# 设置进行压缩的最小文件大小，小于这个值的文件不会被压缩
gzip_min_length 1k;
# 进行压缩的文件类型
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript image/svg+xml;
# 是否在 HTTP header 中添加 Vary: Accept-Encoding
gzip_vary on;
# 不对特定的 User-Agent 进行 gzip 压缩, 兼容IE6
gzip_disable "MSIE [1-6]\.";
```
