---
title: 使用stub配合laravel-excel实现后台运行的excel下载中心
date: 2021-12-08 11:27:44
tags: [2021,laravel,stub]
categories: [代码]
top_img: /img/stub.jpeg
cover: /img/stub.jpeg
---


# 创建模板stub

>建一个stub模板,用于下面使用

在resources下新建stubs目录用于存储
新建一个query-download.stub
```php
<?php

namespace DummyNamespace;

use App\Exports\QueryExport;

class DummyClass extends QueryExport
{

    public function headings(): array
    {
        //表头
         return [
             //
         ];
    }

    public function columns($row): array
    {
        //每一列的数据
         return [
             //
         ];
    }

    public function stringColumns(): array
    {
        //要转为字符串的列标
         return [
            //
         ];
    }

}

```
## stub占位符介绍
> stub文件中会有很多占位符,下面说几个,还有很多占位符,后面再熟悉一下

* DummyNamespace : 命名空间
* DummyRootNamespace : 根命名空间 
* DummyClass : 类名

# 创建command命令

```php
 php artisan make:command Make/Download
```

生成好的新的command,需要如下操作

* 修改继承GeneratorCommand类并必须实现getStub方法
* 移除自动生成的`protected $signature`改为`protected $name`,其中$name就是最后要执行的命令了

下面直接上代码

```php
<?php

namespace App\Console\Commands\Make;

use Illuminate\Console\GeneratorCommand;
use Symfony\Component\Console\Input\InputOption;

class Download extends GeneratorCommand
{
	/**
	 * 运行的命令
	 *
	 * @var string
	 */
	protected $name = 'make:download';

	/**
	 * The console command description.
	 *
	 * @var string
	 */
	protected $description = '生成下载中心';

	/**
	 * 这里是声明生成的文件类型,在新建成功时,命令行会显示xxx created successfully.这里的xxx在这就是Download
	 *
	 * @var string
	 */
	protected $type = 'Download';


	/**
	 * 文件模板的地址,这里就是第一步新建的stub文件的路径
	 * @return string
	 */
	public function getStub(): string
	{
		if ($this->option('array')) {
			return base_path('resources/stubs/array-download.stub');
		}else{
			return base_path('resources/stubs/query-download.stub');
		}
	}

	/**
	 * 获取文件的默认的命名空间,因为文件打算建立在app下的Exports目录下,所以修改了一下这个文件,若要生成在app下,可不重写此方法
	 * Get the default namespace for the class.
	 *
	 * @param  string  $rootNamespace
	 * @return string
	 */
	protected function getDefaultNamespace($rootNamespace): string
	{
		return $rootNamespace . '\Exports';
	}

	/**
	 * 定义可输入的参数
	 * Get the console command options.
	 *
	 * @return array
	 */
	protected function getOptions()
	{
		return [
			//VALUE_NONE 不接受选项的输入,默认值
			//VALUE_REQUIRED 必须传入一个值
			//VALUE_OPTIONAL 可以传值也可以不传值
			//VALUE_IS_ARRAY 可以传多个值 --dir=/foo --dir=/bar
			//VALUE_NEGATABLE 可以正值也可以负值
			['query', '', InputOption::VALUE_NONE, 'Generate an export for a query.'],
			['array', 'a', InputOption::VALUE_NONE, 'Generate an export for an array.']
		];
	}
}


```

## 其他可用方法

```php
    protected function buildClass($name)
    {
        //可以根据传入的参数,自定义其他操作
    }
    
        
    protected function replaceNamespace(&$stub, $name)
    {
       //可以修改底层占位符和自定义占位符
    }
```


## 使用

### query类型测试

执行生成命令,生成query类型的下载器
![](/images/16395675015323.jpg)
![](/images/16395675390875.jpg)

### array类型测试

执行生成命令,生成array类型的下载器

![](/images/16395676227417.jpg)
![](/images/16395676560450.jpg)

ok,就酱

