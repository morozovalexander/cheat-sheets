## symfony 3.x

1.twig 中获取GET参数

    app.request.query.get('keyword')

2.控制器中获取登录用户信息

     $this->getUser();
等价于

    $this->get('security.token_storage')
    ->getToken()
    ->getUser(); 

3.用户密码加密

    /**
     *
     * @var \Symfony\Component\Security\Core\Encoder\UserPasswordEncoder
     */
    $encoder = $this->get('security.password_encoder');

校验密码:

    $encoder->isPasswordValid($user, $userForm->getPassword())；
密码加密:

    $password = $encoder->encodePassword($user, $userForm->getPlainPassword());

4.twig中使用js模板引擎，渲染数据，解决 {{ }} 冲突，使用verbatim。

    {% verbatim %}
	    <ul>
	    {% for item in seq %}
	    <li>{{ item }}</li>
	    {% endfor %}
	    </ul>
    {% endverbatim %}
5.文件上传相关

    $file = $request->files->get('file'); //获取上传的文件
    if($file instanceof UploadedFile){
        //不为空
    	$filename = $this->get('kit.file_uploader')->upload($file, 'file');
    }else{
   		//未上传
    }

6.Doctrine生命周期

    use Doctrine\ORM\Mapping as ORM;
	// Entity类前面
    @ORM\HasLifecycleCallbacks()
	// 类中的方法
	/**
     * @ORM\PrePersist()
     */
    public function prePersist()
    {
        if($this->getCreateAt() == null){
            $this->setCreateAt(new \DateTime());
        }
        $this->setUpdateAt(new \DateTime());
    }
    /**
     * @ORM\PreUpdate()
     */
    public function preUpdate()
    {
        $this->setUpdateAt(new \DateTime());
    }
7.Entity的filed设置

    @ORM\Column(name="filed_name", type="string", length=64, nullable=true, options={"default" : "default_value", "comment": "字段注释"})

- nullable设置是否可以为null
- default设置字段的默认值
- comment字段的注释

8.常用Command命令

- 查看commad列表(public)

		 php bin/console list

- 清除缓存
		
		php bin/console cache:clear
		php bin/console cache:clear --env=prod --no-debug  //清除生产环境的缓存
- 生成bundle
		
		php bin/console generate:bundle
- 生成Entity

		php bin/console doctrine:generate:entity
- 生成crud操作代码
		
		php bin/console doctrine:generate:crud
- 查看service的列表

		php bin/console  debug:container
	- 查看monolog相关的service
			
			 php bin/console  debug:container monolog

			 Select one of the following services to display its information:
			  [0 ] monolog.activation_strategy.not_found
			  [1 ] monolog.handler.fingers_crossed.error_level_activation_strategy
			  [2 ] monolog.processor.psr_log_message
			  [3 ] monolog.handler.main
			  [4 ] monolog.handler.console
			  [5 ] monolog.logger.request
			  [6 ] monolog.logger.cache
			  [7 ] monolog.logger.translation
			  [8 ] monolog.logger.templating
			  [9 ] monolog.logger.profiler
			  [10] monolog.logger.php
			  [11] monolog.logger.event
			  [12] monolog.logger.router
			  [13] monolog.logger.security
			  [14] monolog.logger.doctrine
			  [15] monolog.logger.assetic
			  [16] monolog.handler.null_internal
			 > 3
			3
			
			Information for Service "monolog.handler.main"
			==============================================
			
			 ------------------ -------------------------------
			  Option             Value
			 ------------------ -------------------------------
			  Service ID         monolog.handler.main
			  Class              Monolog\Handler\StreamHandler
			  Tags               -
			  Calls              pushProcessor
			  Public             yes
			  Synthetic          no
			  Lazy               no
			  Shared             yes
			  Abstract           no
			  Autowired          no
			  Autowiring Types   -
			 ------------------ -------------------------------

9.数据库相关

- 根据配置创建数据库

		php bin/console doctrine:database:create

- 执行更新数据库操作前，打印SQL
		
		php bin/console doctrine:schema:update --dump-sql

- 执行更新数据库

		php bin/console doctrine:schema:update --force


