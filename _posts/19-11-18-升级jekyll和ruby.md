---
title: 升级jekyll和ruby
date:  19-11-18 5:56 +08
---

# 升级 jekyll，安装新版 ruby

使用 jekyll 创建新项目时，弹出一大段提示，输入密码一直不行（密码真没错）

```sh
jekyll new demo-test

# 弹出如下一大段提示
Your user account isn't allowed to install to the system RubyGems.
  You can cancel this installation and run:

      bundle install --path vendor/bundle

  to install the gems into ./vendor/bundle/, or you can enter your password
  and install the bundled gems to RubyGems using sudo.

  Password: 
```

```sh
# 系统的 RubyGems 不允许操作，那就升级或重新安装下 jekyll 吧
gem install jekyll

# 弹出如下提示，需要 ruby 2.4.0 以上版本
ERROR:  Error installing jekyll:
	jekyll-sass-converter requires Ruby version >= 2.4.0.


# 查看 ruby 版本
ruby -v

# 打印的信息，v2.3，那就升级下
ruby 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin18]


# 使用 homebrew 安装
brew install ruby


# 安装完有如下一大段提示
==> ruby
By default, binaries installed by gem will be placed into:
  /usr/local/lib/ruby/gems/2.6.0/bin

You may want to add this to your PATH.

ruby is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have ruby first in your PATH run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile

For compilers to find ruby you may need to set:
  export LDFLAGS="-L/usr/local/opt/ruby/lib"
  export CPPFLAGS="-I/usr/local/opt/ruby/include"

For pkg-config to find ruby you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/ruby/lib/pkgconfig"


# 照做即可，将那四行复制到 .bash_profile 文件。查看现在版本
ruby -v

# 打印的信息
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-darwin18]


# 但 jekyll 命令还不能使用
jekyll -v

# 提示找不到命令
-bash: /usr/local/bin/jekyll: No such file or directory


# 查看 gem 安装的文件位置
gem environment

# 打印的信息很长，前四行如下，有个安装目录
RubyGems Environment:
  - RUBYGEMS VERSION: 3.0.6
  - RUBY VERSION: 2.6.5 (2019-10-01 patchlevel 114) [x86_64-darwin18]
  - INSTALLATION DIRECTORY: /usr/local/lib/ruby/gems/2.6.0


# 把该安装目录添加到 .bash_profile，完成✅
export PATH="/usr/local/lib/ruby/gems/2.6.0/bin:$PATH"
```

# gem 卸载默认的模块报错

```sh
# 卸载 bundler
gem uninstall bundler

# 提示 bundler 是默认的，不能卸载
ERROR:  While executing gem ... (Gem::InstallError)
    gem "bundler" cannot be uninstalled because it is a default gem

# 查看版本
gem list | grep bundler

# 提示如下，有两个默认版本
bundler (default: 2.0.2, default: 1.17.3)

# 进入 gem 的默认模块目录
cd /usr/local/Cellar/ruby/2.6.5/lib/ruby/gems/2.6.0/specifications/default

# 将 bundler 移出默认文件夹
mv bundler-1.17.3.gemspec ..

# 再看一下
gem list | grep bundler

# 已经不是默认版本，可以愉快移除了👌
bundler (2.0.2, 1.17.3)


```