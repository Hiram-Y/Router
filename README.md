# Router
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
