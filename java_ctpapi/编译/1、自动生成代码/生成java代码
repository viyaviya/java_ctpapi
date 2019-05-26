##生成交易api的Java可用代码
####1、编写文件
    建立文件thosttraderapi.i 
    内容如下
    %module(directors="1") thosttradeapi 
    %{ 
    #include "ThostFtdcTraderApi.h"
    #include "iconv.h"
    %}
    
    %typemap(out) char[ANY], char[] {
        if ($1) {
            iconv_t cd = iconv_open("utf-8", "gb2312");
            if (cd != reinterpret_cast<iconv_t>(-1)) {
                char buf[4096] = {};
                char **in = &$1;
                char *out = buf;
                size_t inlen = strlen($1), outlen = 4096;
    
                if (iconv(cd, in, &inlen, &out, &outlen) != static_cast<size_t>(-1))
                    $result = JCALL1(NewStringUTF, jenv, (const char *)buf);
                iconv_close(cd);
            }
        }
    }
    
    %feature("director") CThostFtdcTraderSpi; 
    %ignore THOST_FTDC_VTC_BankBankToFuture;
    %ignore THOST_FTDC_VTC_BankFutureToBank;
    %ignore THOST_FTDC_VTC_FutureBankToFuture;
    %ignore THOST_FTDC_VTC_FutureFutureToBank;
    %ignore THOST_FTDC_FTC_BankLaunchBankToBroker;
    %ignore THOST_FTDC_FTC_BrokerLaunchBankToBroker;
    %ignore THOST_FTDC_FTC_BankLaunchBrokerToBank;
    %ignore THOST_FTDC_FTC_BrokerLaunchBrokerToBank;  
    %feature("director") CThostFtdcTraderSpi; 
    %include "ThostFtdcUserApiDataType.h"
    %include "ThostFtdcUserApiStruct.h" 
    %include "ThostFtdcTraderApi.h"  
####2、准备

    安装swig环境 

  [下载swig](https://sourceforge.net/projects/swig/files/swigwin/swigwin-2.0.11/swigwin-2.0.11.zip/download?use_mirror=jaist)
  
    安装jdk64位
    安装libiconv库。这个库主要适用于字节编码转换，因为CTP的结算单信息是GB2312编码，而Java采用UTF-8编码，如果不进行字节转换，得到的结算单信息中的中文将会是乱码。 
    git上已提供
    安装vs2013
    从CTP官网上下载CTP API 64位
    获取文件清单：
                error.dtd
                error.xml
                ThostFtdcMdApi.h
                ThostFtdcTraderApi.h
                ThostFtdcUserApiDataType.h
                ThostFtdcUserApiStruct.h
                thostmduserapi.dll
                thostmduserapi.lib
                thosttraderapi.dll
                thosttraderapi.lib

####3、得到源码
    建立src文件夹
    运行命令 其中包名为ctp.thosttraderapi ，可自定义包名替换
    swig.exe -c++ -java -package ctp.thosttraderapi -outdir src -o thosttraderapi_wrap.cpp thosttraderapi.i