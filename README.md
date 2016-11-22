# Router
路由
通过路由的方式实现页面之间的跳转，支持拦截器功能，即在跳转某些页面时通过判断是否满足某些条件来决定是否进行跳转，如是否需要登录等.
WIKI：https://github.com/Hiram-Y/Router/wiki/%E8%B7%AF%E7%94%B1
测试

    public class RouterTest extends AndroidTestCase {
      public void test_basic_url() {
          Router.clear();
          Router.put("/user/:name/", CoreActivity.class);

          Router router = Router.getInstance(getContext())
                                .url("/user/:sky");
          router.tryToMatch();

          Intent intent = router.getIntent();
          Assert.assertEquals("sky", intent.getStringExtra("name"));
      }


      public void test_basic_param() {
          Router.clear();
          Router.put("/user/:name", CoreActivity.class);
          Router router = Router.getInstance(getContext())
                                .url("/user/")
                                .addParam("name", "sky");
          router.tryToMatch();
          Intent intent = router.getIntent();
          Assert.assertEquals("sky", intent.getStringExtra("name"));
      }

      public void test_basic_param2() {
          Router.clear();
          Router.put("/user/:name/:age?", CoreActivity.class);
          Router router = Router.getInstance(getContext())
                                .url("/user/")
                                .addParam("name", "sky")
                                .addParam("age", "4");
          router.tryToMatch();
          Intent intent = router.getIntent();
          Assert.assertEquals("sky", intent.getStringExtra("name"));
          Assert.assertEquals("4", intent.getStringExtra("age"));
      }
    }

