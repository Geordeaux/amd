---
id: binance_android_component_architecture
title: Binance-Android Component Architecture
hide_title: true
---

# Binance-Android Component Architecture

### Contracts ：zhenqiao.li@binance.com

![img](https://confluence.toolsfdg.net/download/attachments/38200493/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8B%E5%8D%884.15.57.png?version=1&modificationDate=1618215510000&api=v2)

![img](https://confluence.toolsfdg.net/download/attachments/38200493/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8B%E5%8D%884.19.06.png?version=1&modificationDate=1618215566000&api=v2)

### Business module:

The business module is packaged in the module-xxx file directory, including the current business xxx-api module, xxx-internal module, and sample module

Using Hilt dependency injection, we can easily communicate and interact between business modules only by relying on the API layer interface method definition. The final interface implementation is determined by packaging the specified business module code in the shell project into the apk. The boundaries between business modules are clear, and the way of dependency is clear,

The communication between business modules is better oriented to interface programming, avoiding indirect cyclic dependencies between business modules, and reducing the recompilation problems caused by the introduction of excessive code exposure and business code changes caused by the dependence between business modules on the internal implementation layer.

Therefore, we agree that business modules only rely on the api layer for communication and interaction, and each business module is divided into at least two layers: xxx-api, xxx-internal, and the internal layer depends on the api layer

### xxx-api:

The current business module needs to be exposed to other business modules, including: interface, method declaration definition, constant declaration, data class declaration, etc.

### xxx-internal:

The core business code of the business module depends on the api layer and implements the specific implementation of the interfaces and methods in the above api layer.

For the case where there are more subdivided services in a relatively large business module, the internal layer can be subdivided into: xxx-internal-a, xxx-internal-b, xxx-internal-c
At the same time, the basic public code sdk layer of each subdivided business module can be separated from the above internal layer. The above internal-a, internal-b, internal-c, etc. can directly rely on xxx-sdk

### sample:

The independent packaging and running of the shell project of the business module needs to rely on the internal layer of the module and the internal layer of other modules that need to be packaged. This module is mainly to facilitate developers to quickly package, compile and debug the business module, reduce the time-consuming packaging of the entire project, etc. It is not mandatory, and may not be required.

### Dependency rules:

1. The internal layer is not allowed to rely on any internal layer, only the api layer and lib library are allowed;
2. The lib library is not allowed to rely on any internal layer (including the sdk layer that may exist), except for lib-base and lib-common, which can rely on the api layer. In principle, other lib libraries should not rely on the api layer;
3. The api layer is not allowed to rely on any internal layer (including the possible sdk layer), and the api layer is also not allowed to rely on lib-base and lib-cmmon. In principle, it should not rely on other lib libraries;
4. The packaged shell project (including the sample shell project in the module) can rely on any required api layer, internal layer, and lib library;
5. If there are many subdivided businesses under the first-level module, there is an SDK layer, which does not allow cross-level modules to be directly dependent on use;

### Resource:

The resource file used in the module should be placed inside the module, if it is a universal resource file, it can be placed in lib-base;

The resource files used in the module, including layout, drawable, color, style, string, anim, dimen, etc., must use the module name or business abbreviation to ensure a uniform prefix;

### Why do this & Target :

1.The common basic components or business are extracted and sunk into the lib library or Git repository, which can be easily used by each business layer and other project dependencies.

2.Each business code sinks to its own module-xxx module. The code between business modules is isolated and referenced, and separated from the App packaging project, without interference from each other.It is also very convenient to make SDK and transfer it

to Git repository in the future.

3.The business module is further subdivided into xxx-api, xxx-internal, xxx-sdk, etc. The first is for convenient communication and interaction with other business modules; the second is that the internal xxx-internal module can provide a variety of business variants,

such as xxx-binance-internal, xxx-play-internal, etc., so as to choose different variants of business logic when assembling and packaging shell APPS.

4.Separate the shell engineering Application module which is responsible for packaging and assembling at the upper level, and select the business module which needs to be packaged according to the packaging channel, so as to realize the free assembly of various APKs

based on a set of engineering codes.

***The Base component library is named in lib-xxx form, and mainly includes business independent Base class, network Base library, Hybrid, Router&lib-modules, general utility class, general UI component class, logging embedding point, tracking and monitoring, etc.**

***The business module is wrapped in the module-xxx file directory, including the current business xxx-api module, xxx-internal module, sample module(may not have)**

**xxx-api :** It mainly refers to the interface, service and other method definitions exposed by the current business module to other modules, other constants such as route key, part of bean classes, Shared data LiveData, module configuration management, etc

**xxx-internal：**The main business code for the current business module

**sample：**

The independent packaging shell project of the business module depends on the internal layer of the module and the internal layer of other modules that need to be packaged into it. This module is mainly for the convenience of developers to quickly pack the business module compilation and debugging, reduce the time spent on packaging the whole project, etc., do not make mandatory requirements, may not have.

### Shell Engineering App Profile：

Responsible for assembling and packaging required business into APK and its variants, initialization of each module, debugging tools, etc.

**Current situation：**

Now in the bnb-android project, most of the android engineering business code and package configurations are unified under the module an app directory, app module build. Gradle script contains both the pack build configuration, for the most part,

also includes the upper business dependencies of dependence, packaged mix build configuration script and business, script bloated, responsibility logic is not clear, change frequently.

Secondly, the debug tool is also stored in the APP directory, so that the script also contains the dependency configuration items of the debug tool.

**Target：**

In the bnb-android project, the shell project Portal, which is specifically used for configuring build packaging, is independently responsible for packaging build scripts and packaging business modules to generate APK as required.

**Benefit：**

1. The project is more clearly stratified. Shell engineering is at the top level with clear responsibilities and unified management of all business modules at the next level. Its scripts are only responsible for packaging the build configuration and do not participate in the dependency configuration of specific business modules. It is also convenient to dynamically configure the business module dependencies required in the packaged APK in the script, especially later if there is a need to package the corresponding business module by channel.
2. As the project development of modular, separate the shell from the app module project, after the app module can temporarily first as a basic business library, subsequent to split from the other business module can temporarily to rely on the library, greatly convenient business module from the app incrementally stripping, componentized process error and migration to reduce costs, improve the efficiency of modular development.
3. Speed up compilation. With the construction of shell engineering, after other business modules are removed from app module, the compilation process of the project only needs to compile the module whose corresponding code is involved in modification and its dependent modules, instead of compiling app module completely. With the improvement of the componentization of the project, the APP module will eventually be completely discarded and deleted.

### Module profile：

lib-base: Base system components baseActivity, baseFragment, baseDialog, generic Ext, and so on

lib-modules: The intermodule communication component and may be subsequently merged with the routing component

lib-hybrid: Business common H5 module foundation and may extend other mixed development patterns later

lib-utils: Common tool library

lib-ui: Common UI library

lib-net：Business common network module foundation

module-launcher: App startup related, splashActivity, mainActivity, Application implementation, home page of app and the skeleton of each TAB, etc

module-market: Market TAB business, market prices, etc

module-trade: Trade TAB business

module-futures: Futures TAB business

module-c2c：C2C business

module-convert：Convert business

module-ocbs: Ocbs business

Other business scenarios that may need to be moduleized: capital page, coin safe, login, setup, kyc, account, etc

### Note**：**

1. Try not to store your business runtime data in your App class. Use intents, SP, files, or LiveData to share variables.
2. Also try not to store business runtime data in the AppData class, which is only a temporary transition class and may be deleted later.
3. Try to use the Context.getContext () method to get the global Context object, but it is not recommended to use the App.getApplication() method, because the later split business module cannot refer to the App class. The use of ContextUtil.getContext ().getString() is not recommended, will cause Crowdin's multilingual languages problems.
4. Try not to add new code to the library of lib-data, which was created by colleagues in 18-year to share some classes. With the development of project componentization, this library will be replaced and deleted in the future.
5. To get version number, use AppUtil.getVerisonCode (); to get version name, use AppUtil.getVerisonName (); to get if the App is compiled in debug mode, use AppUtil.isdebug ().
6. The name of layout file, drawable resource or file, key value of String resource, name of style resource, etc. of each business module should be included the name of the business module you are currently in as far as possible.
These names of the current business module should not be the same as that of other modules.Otherwise, the resource file or key with the same name will be overwritten by other modules with the same name when APP packaging resource file is merged, causing errors.