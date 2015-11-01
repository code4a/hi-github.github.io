---
layout: post
title: Android Volley
categories: [net]
---


# Android Volley 

1. ### 特点

	> 1. 通信更快，更简单
	> 2. Get，Post网络请求及网络图像的高效率异步处理请求
	> 3. 排序
	> 4. 网络请求的缓存
	> 5. 多级别取消请求
	> 6. 和Activity生命周期的联动
	> 7. 缺点是不适合上传和下载
	
2. ### 为什么使用Volley

	> 1. 功能上：<br/>a. 高效的Get/Post方式的数据请求交互<br/>b. 网络图片加载和缓存
	> 2. 其他：<br/> a. 谷歌官方推出<br/>b. 性能很稳定和强劲
	
3. ### Volley的使用

	1. #### get和post请求方式的使用

		a. 挑选合适的对象：自带三种
		
			StringRequest      // 应用场景：针对请求数据返回的结果类型不确定,涵盖以下两种
			JsonObjectRequest  // 
			JsonArrayRequest   //
			
		b. 回调使用
		
			封装常用回调类， 请求成功，失败等
			
	2. #### 网络请求队列建立和取消队列请求

		a. 建立请求队列(全局)
		
		b. 取消请求队列
		
	3. #### 与Activity生命周期联动

		a. 特点	
		
			可在Activity销毁时候，同时关闭请求
			
		b. 关键点
		
			设置Tag标签， onStop() 里执行取消请求
			
	4. #### Volley的简单的二次回调封装

		a. 优势： 全局使用一个方式，可控，可自定义定制需求。管理方便，灵活
		
4. ### 使用Get/Post方式请求数据

	1. #### 建立的基本步骤

		a. 在application 中建立全局请求队列
		
			package jiang.gm;

			import com.android.volley.RequestQueue;
			import com.android.volley.toolbox.Volley;

			import android.app.Application;

			public class GmApplication extends Application {

				// 创建全局请求队列
				public static RequestQueue mRqueues;

				@Override
				public void onCreate() {
					super.onCreate();
					mRqueues = Volley.newRequestQueue(getApplicationContext());
				}

				public static RequestQueue getRequestQueue(){
					return mRqueues;
				}
			}

		b. 创建对应的请求方式(Get)
		
			StringRequest sRequest = new StringRequest(Method.GET,
				NetConst.HOST_URL, new Listener<String>() {

					@Override
					public void onResponse(String response) {
						// TODO Auto-generated method stub

					}
				}, new Response.ErrorListener() {

					@Override
					public void onErrorResponse(VolleyError error) {
						// TODO Auto-generated method stub

					}
				});
			sRequest.setTag("GmGetTAG");
			GmApplication.getRequestQueue().add(sRequest);
		
		c. 创建对应的请求方式(Post)
		
			StringRequest sRequest = new StringRequest(Method.POST, NetConst.HOST_URL, new Listener<String>() {
	            @Override
	            public void onResponse(String response) {
	            }
        	}, new Response.ErrorListener() {
	            @Override
	            public void onErrorResponse(VolleyError error) {
	            }
        	}) {
	            @Override
	            protected Map<String, String> getParams() throws AuthFailureError {
	                Map<String, String> params = new HashMap<String, String>();
	                params.put("", "");
	                return params;
	            }
        	};
        	sRequest.setTag("GmPostTAG");
        	GmApplication.getRequestQueue().add(sRequest);

	2. #### 依据TAG取消对应的请求队列
	
		    @Override
		    protected void onStop() {
		        super.onStop();
		        GmApplication.getRequestQueue().cancelAll("GmGetTAG");
		        GmApplication.getRequestQueue().cancelAll("GmPostTAG");
		    }

5. ### 简单的需求定制

		public class MainActivity extends SherlockActivity {

		    private static final String INSTAGRAM_CLIENT_ID = "Your INSTAGRAM CLIENT ID";
		    private static final Object TAG = new Object();
		    private static final String LOG = "VOLLEY-SAMPLE";
		    private RequestQueue mQueue;
		    private LinearLayout mBase;
		    private MenuItem mRefreshMenu;
		    private volatile int mTotalLoadedImgs = 0;
		    private ImageLoader mImageLoader;

		    private void toast(int id) {
		        String text = getResources().getString(id);
		        Toast.makeText(getApplicationContext(), text, Toast.LENGTH_LONG).show();
		    }

		    private void startLoadingAnim() {
		        if (mRefreshMenu != null) {
		            Log.i(LOG, "===== start loading");
		            ImageView iv = (ImageView) mRefreshMenu.getActionView();
		            Animation rotation = AnimationUtils.loadAnimation(this,
		                    R.anim.refresh_rotate);
		            rotation.setRepeatCount(Animation.INFINITE);
		            iv.startAnimation(rotation);
		        }
		    }

		    private void stopLoadingAnim() {
		        if (mRefreshMenu != null) {
		            Log.i(LOG, "===== stop loading");
		            ImageView iv = (ImageView) mRefreshMenu.getActionView();
		            iv.setImageResource(R.drawable.ic_action_refresh);
		            iv.clearAnimation();
		        }
		    }

		    @Override
		    protected void onCreate(Bundle savedInstanceState) {
		        super.onCreate(savedInstanceState);
		        requestWindowFeature(Window.FEATURE_INDETERMINATE_PROGRESS);
		        setContentView(R.layout.photo_stream);

		        mBase = (LinearLayout) findViewById(R.id.base);
		        mQueue = Volley.newRequestQueue(getApplicationContext());// thread
		                                                                 // pool(4)
		        mImageLoader = new ImageLoader(mQueue, new ImageCache() {
		            private final LruCache<String, Bitmap> mCache = new LruCache<String, Bitmap>(10);
		            public void putBitmap(String url, Bitmap bitmap) {
		                mCache.put(url, bitmap);
		            }
		            public Bitmap getBitmap(String url) {
		                return mCache.get(url);
		            }
		        });
		        startLoadingAnim();
		        refreshDatas();
		    }

		    private void refreshDatas() {
		        String url = "https://api.instagram.com/v1/media/popular?client_id="
		                + INSTAGRAM_CLIENT_ID;
		        JsonObjectRequest jsonRequet = new JsonObjectRequest(Method.GET, url,
		                null, new Listener<JSONObject>() {
		                    public void onResponse(JSONObject result) {
		                        try {
		                            int code = parseJson(result);
		                            if (code != 200) {
		                                toast(R.string.server_error);
		                            }
		                        } catch (JSONException e) {
		                            e.printStackTrace();
		                            toast(R.string.json_error);
		                        }
		                    }
		                }, new Response.ErrorListener() {
		                    public void onErrorResponse(VolleyError error) {
		                        toast(R.string.connection_error);
		                    }
		                });
		        jsonRequet.setTag(TAG);
		        mQueue.add(jsonRequet);
		    }

		    private int parseJson(JSONObject root) throws JSONException {
		        boolean useNetworkImageView = true;
		        int code = root.getJSONObject("meta").getInt("code");
		        if (code == 200) {
		            int[] resTexts = { R.id.photo_text1, R.id.photo_text2,
		                    R.id.photo_text3, R.id.photo_text4, R.id.photo_text5,
		                    R.id.photo_text6 };
		            int[] resImgs = { R.id.photo1, R.id.photo2, R.id.photo3,
		                    R.id.photo4, R.id.photo5, R.id.photo6 };
		            int resIndex = 0;
		            LayoutInflater inf = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);

		            JSONArray arr = root.getJSONArray("data");
		            final int len = arr.length();
		            RelativeLayout rl = (RelativeLayout) inf.inflate(
		                    R.layout.photo_item, null);
		            mBase.addView(rl);
		            for (int i = 0; i < len; i++, resIndex++) {
		                JSONObject json = arr.getJSONObject(i);
		                String text = null;
		                try {
		                    JSONObject caption = json.getJSONObject("caption");
		                    if (caption != null) {
		                        text = caption.getString("text");
		                    } else {
		                        text = "...";
		                    }
		                } catch (Exception e) {
		                    e.printStackTrace();
		                    Log.e(LOG, json.toString());
		                    text = "...";
		                }
		                String imgUrl = json.getJSONObject("images")
		                        .getJSONObject("low_resolution").getString("url");

		                NetworkImageView niv = (NetworkImageView) rl
		                        .findViewById(resImgs[resIndex]);
		                if (niv == null) {
		                    rl = (RelativeLayout) inf
		                            .inflate(R.layout.photo_item, null);
		                    mBase.addView(rl);
		                    resIndex = 0;
		                    niv = (NetworkImageView) rl.findViewById(resImgs[resIndex]);
		                }

		                TextView tv = (TextView) rl.findViewById(resTexts[resIndex]);
		                tv.setText(text);
		                tv.invalidate();

		                if (useNetworkImageView) {
		                    // in case of using NetworkImageView
		                    niv.setImageUrl(imgUrl, mImageLoader);
		                } else {
		                    requestImage(niv, imgUrl, len);
		                }
		            }
		            if (useNetworkImageView) {
		                stopLoadingAnim();
		            }
		        }
		        return code;
		    }

		    private void requestImage(final ImageView niv, final String imgUrl, final int len) {
		        ImageRequest request = new ImageRequest(imgUrl, new Listener<Bitmap>() {
		            @Override
		            public void onResponse(Bitmap bm) {
		                niv.setImageBitmap(bm);
		                mTotalLoadedImgs++;
		                if (len == mTotalLoadedImgs) {
		                    stopLoadingAnim();
		                    mTotalLoadedImgs = 0;
		                }
		            }
		        }, 0, 0, Config.ARGB_8888, new ErrorListener() {
		            public void onErrorResponse(VolleyError arg0) {
		                arg0.printStackTrace();
		                mTotalLoadedImgs++;
		                if (len == mTotalLoadedImgs) {
		                    stopLoadingAnim();
		                    mTotalLoadedImgs = 0;
		                }
		            }
		        });
		        request.setTag(TAG);
		        mQueue.add(request);
		    }

		    public void onStop() {
		        super.onStop();
		        mQueue.cancelAll(TAG);
		    }

		    @Override
		    public boolean onCreateOptionsMenu(Menu menu) {
		        getSupportMenuInflater().inflate(R.menu.main, menu);
		        mRefreshMenu = menu.findItem(R.id.action_refresh);
		        ImageView iv = (ImageView) mRefreshMenu.getActionView();
		        LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
		        iv = (ImageView) inflater.inflate(R.layout.actionbar_refresh, null);
		        iv.setId(R.id.action_refresh);
		        iv.setOnClickListener(new OnClickListener() {
		            public void onClick(View v) {
		                mQueue.cancelAll(TAG);
		                mBase.removeAllViews();
		                stopLoadingAnim();
		                startLoadingAnim();
		                refreshDatas();
		            }
		        });
		        mRefreshMenu.setActionView(iv);
		        startLoadingAnim();
		        return true;
		    }

		    @Override
		    public boolean onOptionsItemSelected(MenuItem item) {
		        return true;
		    }
		}
	
		
	