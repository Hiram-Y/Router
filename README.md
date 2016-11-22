# Router
路由
通过路由的方式实现页面之间的跳转，支持拦截器功能，即在跳转某些页面时通过判断是否满足某些条件来决定是否进行跳转，如是否需要登录等.

支持以下几种配置方式

"/login/:name/" //其中name为必传参数
"/login/:name/:age?" //其中name为必传参数，age为可选参数
"/login/:name/@user" //其中name与user均为必传参数，但是user为可序列化对象
页面跳转是传递参数的方式

router.url("/login/sky) //通过url传递，只有一个参数 name=sky
router.url("/login/sky/33) //错误，由于age为可选参数，所以只能通过addParam的方式传递所有参数
router.url("/login).addParam("name", "sky")).addParam("age", "33") //正确
router.url("/login).addParam("name", "sky")) //通过addParam的方式来传递参数
当含有可选参数时必须通过addParam的方式来传递所有参数，可序列化类型的参数也必须通过addParam的方式传递
配置路由信息事例

Router.put("/loan/", LoanActivity.class, loginInterceptor);
Router.put("/loan/user/@user/", LoanActivity.class);
Router.put("/loan/paged/:page/:num?", PagedLoanActivity.class);
页面跳转事例

//基本类型
Router.getInstance(mFragment.getContext())
              .url("/loan/")
              .open();

//传递可序列化参数              
Router.getInstance(mFragment.getContext())
              .url("/loan/user/")
              .addParam("user", user)
              .open();

//带有必传参数以及可选参数的页面跳转
Router.getInstance(mFragment.getContext())
              .url("/loan/paged/")
              .addParam("page", "1")
              .open();

//需要返回结果时的跳转方式
Router.getInstance(mFragment.getContext())
              .url("/loan/")
              .needResult(true)
              .resultCode(111)
              .open();
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

