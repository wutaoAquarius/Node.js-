# AspNet Core 入门笔记
- 什么是AspNet Core？
- AspNet Core 与Asp Net区别？
- AspNet Core 为什么能够跨平台？
  1. CreateWebHostBuild 又是怎么样去创建一个 http服务器?
  2. http服务器又是如何实现 监听>解析>转发>回发 这一过程？
- 什么是中间件？
  1. 中间件的实现原理？
  2. 如何利用中间件进行拓展？


## 什么是AspNet Core；


## AspNet Core与Asp Net 区别
1. 网络请求模型的变化；
以前.NetFramework下的网站都是托管在IIS上面;
AspNet 的网络请求模型为：HTTP请求 > DNS 解析 > 发包到某台服务器的端口 > 服务器上的IIS会有一个监听服务,监听到请求后做解析处理 > 转发 > 调用AspNet代码;
AspNet 接收请求的过程是由IIS帮忙完成的，那也就是说 AspNet Core如果要进行跨平台，第一件事就需要剥离IIS的依赖；
在没有IIS帮助程序进行 监听>解析>转发>回发 的情况下,AspNet Core就需要自己来完成这些事情;

2. 运行环境的变化;
AspNet网站运行需要IIS的进程池才能运行;
而AspNet Core没有了web站点的概念,每个Core程序都是一个控制台程序;每个控制台都是一个独立的进程;
那么控制台程序是如何去完成 请求的 监听>解析>转发>回发 的呢? 控制台在启动时 会执行 CreateWebHostBuild(args).Build().Run()，创建了一个默认的http服务器 kestrel （等同于IIS），完成了 监听>解析>转发>回发;
生成了默认http服务器后,window的IIS就只作为反向代理作用,也可以用nginx作为反向代理来取代iis;
Program.cs
```CS
namespace WebApplication1
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)  
                .UseStartup<Startup>();
    }
}
```
StartUp.cs
```cs
namespace WebApplication1
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.Configure<CookiePolicyOptions>(options =>
            {
                // This lambda determines whether user consent for non-essential cookies is needed for a given request.
                options.CheckConsentNeeded = context => true;
                options.MinimumSameSitePolicy = SameSiteMode.None;
            });


            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();
            app.UseCookiePolicy();

            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "{controller=Home}/{action=Index}/{id?}");
            });
        }
    }
}

```
IApplicationBuilder： 应用程序建造者-Build（）-应用程序的实例---请求就是它来处理的---用中间件来组装的一个Application处理流程；
中间件 middleware 就是一个委托：Func<RequestDelegate, RequestDelegate> middleware

**那么 这个CreateWebHostBuild 又是怎么样去创建一个 http服务器呢? 而http服务器又是如何实现 http服务器 监听>解析>转发>回发 这一过程的呢?**

3. 管道模型的区别；



