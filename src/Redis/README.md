## redis
- [redis windows](redis_windows.md)
- [master slave](master-slave.md)
- redis 设置密码  
	打开redis.conf，找到requirepass去掉前面的#后面改成你想要设置的密码

		requirepass kitlbas_cn
	PHP redis扩展连接redis

		$redis = new Redis();
		$redis->connect($host, $port);
		$redis->auth('kitlbas_cn');
		$redis->set('needpassword', '123456');
		echo $redis->get('needpassword');

	redis-cli 进入也是需要密码的

		C:\Users\Auser>redis-cli
		127.0.0.1:6379>
		127.0.0.1:6379>
		127.0.0.1:6379> get kitlbas
		(error) NOAUTH Authentication required.
		127.0.0.1:6379>
		127.0.0.1:6379> auth kitlbas_cn
		OK
		127.0.0.1:6379>
		127.0.0.1:6379> get kitlbabs
		(nil)
		127.0.0.1:6379>
		127.0.0.1:6379>