**Linux 开机启动脚本**

---

## 方法一： 简单的启动命令

直接在 `/etc/rc.local` 或者 `/etc/rc.d/rc.local` 文件中添加命令。(前者是后者的软连接)

## 方法二： 添加启动项

1. 在 `/etc/init.d` 目录下建立启动脚本，脚本名字可以随便取，开机时会自动执行该目录下的所有脚本文件。
2. 设置脚本文件的执行权限

```sh
sudo chmod +x /etc/init.d/xxx.sh
```

3. 使用 `update-rc.d` 命令将脚本放到启动脚本中

```sh
cd /etc/init.d
sudo update-rc.d xxx.sh defaults 95   # 95 按需更改
```

* 95 是脚本的启动顺序，当有多个脚本文件时，脚本之间可能存在依赖关系，这时需要严格设置好脚本文件的启动顺序。

除了使用 `update-rc.d` ，还可以自己动手在 `rc*.d` 建立软连接。

```sh
ln -s xxx.sh ../rc5.d/S95xxx.sh
```

* 这里的`rc*.d`，* 代表启动级别，在不同启动级别启动，K 开头的脚本文件代表运行级别加载时需要关闭的，S 开头的代表相应级别启动时需要执行，数字代表顺序.

4. 卸载启动脚本

```sh
cd /etc/init.d
sudo update-rc.d -f xxx.sh remove
```

5. 开机自动启动后，还可以进行手动调整

```sh
cd /etc/init.d
./xxx.sh restart    # 执行程序时传入参数
./xxx.sh stop
./xxx.sh start
```

## 其他启动文件

```sh
# ===> bootstrap.sh
# 该脚本时启动引导文件

# ===> init.sh
# 该文件时开机时执行的一系列初始化配置文件
# 可以放在 bootstrap.sh 中，并设置执行顺序
# 这样可以将自己想让机器自动化的部分在开机时完成

```

==TODO==

看一下《实验楼》的课程《系统自动化》

