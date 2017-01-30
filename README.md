#gorb

Gorb is a package built upon [Gorp](https://github.com/go-gorp/gorp) and strongly inspired by [Nap](https://github.com/tsenart/nap). 
It allows load balancing in a round robin style between master and slave databases.

Read queries are executed by slaves.
Write queries are executed by the master.

[Gorp documentation](https://godoc.org/gopkg.in/gorp.v2)

## Usage

```
	func main(){
		dsns := "root:root@tcp(master:3306)/masterpwd;"
		dsns += "root:root@tcp(slave:3306)/slavepwd"
		b, err := gorb.NewBalancer("mysql", gorp.MySQLDialect{"InnoDB", "utf8mb4"}	, dsns)
		if err != nil{
			panic(err)
		}
		err = b.Ping()
		if err != nil{
			panic(err)
		}
		
		// Master is used only for write queries, this is the default value
        b.MasterCanRead(false) 

        // Master is used for write and read queries
        b.MasterCanRead(true)
		
		count, err := b.SelectInt("SELECT COUNT(*) FROM mytable")
		if err != nil{
			panic(err)
		}
		fmt.Println(count)
		
	}
```
## License

Gorb is licensed under the [MIT License](./LICENSE).

