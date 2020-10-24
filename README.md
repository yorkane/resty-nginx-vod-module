# resty-nginx-vod-module

## Demo nginx.conf
> https://github.com/kaltura/nginx-vod-module
```
worker_processes  1;
load_module modules/ngx_http_vod_module.so;
http {
	upstream fallback {
		server fallback.kaltura.com:80;
	}

	server {
		# vod settings
		vod_mode local;
		vod_fallback_upstream_location /fallback;
		vod_last_modified 'Sun, 19 Nov 2000 08:52:00 GMT';
		vod_last_modified_types *;

		# vod caches
		vod_metadata_cache metadata_cache 512m;
		vod_response_cache response_cache 128m;
		
		# gzip manifests
		gzip on;
		gzip_types application/vnd.apple.mpegurl;

		# file handle caching / aio
		open_file_cache          max=1000 inactive=5m;
		open_file_cache_valid    2m;
		open_file_cache_min_uses 1;
		open_file_cache_errors   on;
		aio on;
		
		location ^~ /fallback/ {
			internal;
			proxy_pass http://fallback/;
			proxy_set_header Host $http_host;
		}

		location /content/ {
			root /web/;
			vod hls;
			add_header Access-Control-Allow-Headers '*';
			add_header Access-Control-Expose-Headers 'Server,range,Content-Length,Content-Range';
			add_header Access-Control-Allow-Methods 'GET, HEAD, OPTIONS';
			add_header Access-Control-Allow-Origin '*';
			expires 100d;
		}
	}
}
```