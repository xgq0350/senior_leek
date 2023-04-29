​
1. viper的介绍
viper是go一个配置解决方案的库。
支持各种配置文件，如JSON，TOML, YAML, HCL, envfile和Java属性配置文件
支持监听文件变化以及重新读取配置
支持从环境变量读取配置
支持从远程配置系统(etcd或Consul)读取配置，并能监听远程配置修改
支持从命令行标志Flag读取配置，比如搭配cobra使用

viepr直接使用go get命令安装即可

$ go get github.com/spf13/viper

2.viper的使用
//加载本地配置
func InitInFromLocal() (*viper.Viper, error) {
	home := "./config"
	var v = viper.New()
	//cobra.CheckErr(err)
	fmt.Printf("UserHomeDir", home)
	// Search config in home directory with name ".cobra" (without extension).
	v.AddConfigPath(home)
	v.SetConfigType("toml")
	v.SetConfigName("pro")
	v.AutomaticEnv()
	if err := v.ReadInConfig(); err == nil {
		fmt.Println("Using config file success:", v.ConfigFileUsed())
	} else {
		fmt.Println("Using config file:%s", err)
	}
	fmt.Println(v.GetString("app_name_pro"))
	return v, nil
}
//加载远端配置
func InitConfigFromRemote() error {
	// 远程配置
	viper.AddRemoteProvider("etcd", "http://127.0.0.1:2379", "config/app.yml")
	//v.SetConfigType("json")
	viper.SetConfigFile("app.yml")
	viper.SetConfigType("yml")

	if err := viper.ReadRemoteConfig(); err == nil {
		log.Printf("use config file -> %s\n", viper.ConfigFileUsed())
	} else {
		return err
	}
	return nil
}
//设置默认值
	viper.SetDefault("email", "NAME HERE <EMAIL ADDRESS>")
	viper.SetDefault("license", "apache")
//绑定单个值cobra
	viper.BindPFlag("author", rootCmd.PersistentFlags().Lookup("author"))
	viper.BindPFlag("useViper", rootCmd.PersistentFlags().Lookup("viper"))
//绑定多个值
	viper.BindPFlags(rootCmd.PersistentFlags())
//获取环境变量
    v.AutomaticEnv()
	v.AllowEmptyEnv(true)
	v.SetEnvPrefix("CK")
//监听文件变化

// 监听到文件变化后的回调
v.OnConfigChange(func(e fsnotify.Event) {
  fmt.Println("Config file changed:", e.Name)
  fmt.Println(v.Get("db.redis.passwd"))
})
 
v.WatchConfig()
//从viper写入文件
	viper.SetConfigFile("./hello.yml")
	viper.WriteConfig()
//获取子配置项
    viper.Sub("db")


​