###数据保护(Data Protection)---
layout: post
title: Sample Post
date: 2018-04-14 15:32:24.000000000 +09:00
---

##数据保护API--
文件系统中的文件、keychain中的项，都是加密存储的。当用户解锁设备后，系统通过UDID密钥和用户设定的密码生成一个用于解密的密码密钥，存放在内存中，直到设备再次被锁，开发者可以通过Data Protection API 来设定文件系统中的文件、keychain中的项应该何时被解密。


    1> 文件保护
    {% highlight ruby %}
    /* 为filePath文件设置保护等级 */
    NSDictionary *attributes = [NSDictionary dictionaryWithObject:NSFileProtectionComplete
    forKey:NSFileProtectionKey];
    [[NSFileManager defaultManager] setAttributes:attributes
                                     ofItemAtPath:filePath
                                            error:nil];



    //文件保护等级属性列表
    NSFileProtectionNone                                    //文件未受保护，随时可以访问 （Default）
    NSFileProtectionComplete                                //文件受到保护，而且只有在设备未被锁定时才可访问
    NSFileProtectionCompleteUntilFirstUserAuthentication    //文件收到保护，直到设备启动且用户第一次输入密码
    NSFileProtectionCompleteUnlessOpen                      //文件受到保护，而且只有在设备未被锁定时才可打开，不过即便在设备被锁定时，已经打开的文件还是可以继续使用和写入
    {% endhighlight %}

    2> keychain项保护
    {% highlight ruby %}
    /* 设置keychain项保护等级 */
    NSDictionary *query = @{(__bridge id)kSecClass: (__bridge id)kSecClassGenericPassword,
    (__bridge id)kSecAttrGeneric:@"MyItem",
    (__bridge id)kSecAttrAccount:@"username",
    (__bridge id)kSecValueData:@"password",
    (__bridge id)kSecAttrService:[NSBundle mainBundle].bundleIdentifier,
    (__bridge id)kSecAttrLabel:@"",
    (__bridge id)kSecAttrDescription:@"",
    (__bridge id)kSecAttrAccessible:(__bridge id)kSecAttrAccessibleWhenUnlocked};

    OSStatus result = SecItemAdd((__bridge CFDictionaryRef)(query), NULL);



    //keychain项保护等级列表
    kSecAttrAccessibleWhenUnlocked                          //keychain项受到保护，只有在设备未被锁定时才可以访问
    kSecAttrAccessibleAfterFirstUnlock                      //keychain项受到保护，直到设备启动并且用户第一次输入密码
    kSecAttrAccessibleAlways                                //keychain未受保护，任何时候都可以访问 （Default）
    kSecAttrAccessibleWhenUnlockedThisDeviceOnly            //keychain项受到保护，只有在设备未被锁定时才可以访问，而且不可以转移到其他设备
    kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly        //keychain项受到保护，直到设备启动并且用户第一次输入密码，而且不可以转移到其他设备
    kSecAttrAccessibleAlwaysThisDeviceOnly                  //keychain未受保护，任何时候都可以访问，但是不能转移到其他设备
    {% endhighlight %}

应用实例
把一段信息infoStrng字符串写进文件，然后通过Data Protection API设置保护。
    {% highlight ruby %}
    NSString *documentsPath =[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
    NSString *filePath = [documentsPath stringByAppendingPathComponent:@"DataProtect"];
    [infoString writeToFile:filePath
    atomically:YES
    encoding:NSUTF8StringEncoding
    error:nil];
    NSDictionary *attributes = [NSDictionary dictionaryWithObject:NSFileProtectionComplete
    forKey:NSFileProtectionKey];
    [[NSFileManager defaultManager] setAttributes:attributes
    ofItemAtPath:filePath
    error:nil];

    {% endhighlight %}

设备锁屏（带密码保护）后，即使是越狱机，在root权限下cat读取那个文件信息也会被拒绝!
