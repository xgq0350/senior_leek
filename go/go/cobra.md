​
一、spf13/cobra
spf13/cobra 和 urfave/cli 是Go的优秀命令行工具：

安装cobra

go install github.com/spf13/cobra-cli@latest

二、命令样例
hugo server --port=1313（ hugo：根命令  server：子命令  –port：标志）

package cmd

import (
	"fmt"
	"github.com/fsnotify/fsnotify"
	"github.com/spf13/cobra"
	"github.com/spf13/viper"
	"log"
	"os"
)

var rootCmd = &cobra.Command{
	Use:   "hugo",
	Short: "Hugo is fast start",
	Long: `Huto is very 
Long start
`,
	Run: runRoot,
}

func Execute() {
	if err := rootCmd.Execute(); err != nil {
		fmt.Println(os.Stderr, err)
	}
}

func runRoot(cmd *cobra.Command, args []string) {
	fmt.Printf("execute %s args:%v \n", cmd.Name(), args)
	fmt.Printf("name %s age:%v \n", name, age)
	fmt.Printf("%#v\n", viper.GetString("database.default.connection"))
	fmt.Printf("%#v\n", viper.Sub("database.default").GetString("connection"))

}

var (
	cfgFile     string
	userLicense string
)
var name string
var age int

func init() {

	cobra.OnInitialize(initConfig)
	rootCmd.PersistentFlags().StringVarP(&cfgFile, "config", "c", "", "config file (default is $HOME/.cobra.yaml)")
	rootCmd.PersistentFlags().StringP("author", "a", "YOUR NAME", "author name for copyright attribution")
	rootCmd.PersistentFlags().StringVarP(&userLicense, "license", "l", "", "name of license for the project")
	rootCmd.PersistentFlags().Bool("viper", true, "use Viper for configuration")

	viper.BindPFlag("author", rootCmd.PersistentFlags().Lookup("author"))
	viper.BindPFlag("useViper", rootCmd.PersistentFlags().Lookup("viper"))
	viper.SetDefault("email", "NAME HERE <EMAIL ADDRESS>")
	viper.SetDefault("license", "apache")
	rootCmd.Flags().StringVarP(&name, "name", "n", "100", "input name")
	rootCmd.Flags().IntVarP(&age, "age", "g", 10, "input age")
}

func initConfig() {
	if cfgFile != "" {
		// Use config file from the flag.
		fmt.Println("___________________________")
		viper.SetConfigFile(cfgFile)
	} else {
		// Find home directory.
		home, err := os.UserHomeDir()
		home = "./config"
		cobra.CheckErr(err)
		fmt.Printf("UserHomeDir", home)
		// Search config in home directory with name ".cobra" (without extension).
		viper.AddConfigPath(home)
		viper.SetConfigType("toml")
		viper.SetConfigName("dev")
	}
	if err := viper.ReadInConfig(); err == nil {
		fmt.Println("Using config file success:", viper.ConfigFileUsed())
	} else {
		fmt.Println("Using config file:%s", err)
	}

}

package cmd

import (
	"fmt"
	"github.com/spf13/cobra"
)

var helloCmd = &cobra.Command{
	Use:   "hello",
	Short: "hello 子命令.",
	Long:  "这是一个Hello 子命令",
	Args:  cobra.MinimumNArgs(1),
	Run:   runHello,
}

var study int
var provice string

func init() {
	helloCmd.Flags().StringVarP(&provice, "provice", "p", "100", "input name")
	helloCmd.Flags().IntVarP(&study, "study", "s", 10, "input age")
	rootCmd.AddCommand(helloCmd)

}

func runHello(cmd *cobra.Command, args []string) {
	// TODO 这里处理hello子命令

	fmt.Printf("Hello %s, name %s, age %v, provice %s, study %v", args[0], name, age, provice, study)
}

package cmd

import (
	"github.com/gin-gonic/gin"
	"github.com/spf13/cobra"
	"net/http"
)

var httpCmd = &cobra.Command{
	Use:   "http",
	Short: "http 子命令.",
	Long:  "http 子命令",
	Args:  cobra.MinimumNArgs(1),
	Run:   runHttp,
}

func init() {
	rootCmd.AddCommand(httpCmd)
}

func runHttp(cmd *cobra.Command, args []string) {

	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "pong",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}

2.go.mod 文件下设置 module demo

目录下执行 go build

3.执行命令 demo -h

demo hello -h

​