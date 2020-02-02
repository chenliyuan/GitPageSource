title: RT运行命令
author: 躲不掉的风
date: 2020-02-01 20:20:11
tags:
---
##### robot命令

使用1：运行程序并生成报告

    robot -d Output --loglevel TRACE -v env:$env -v HUB:$hub -i $area SellerCenterUI/TestCase/06Wallet/
    
使用2：重新运行失败的case

    robot -d Output --rerunfailed Output/output.xml --output rerun.xml --loglevel TRACE -v env:$env -v HUB:$hub  -i $area SellerCenterUI/TestCase/01Login/

据robot --help可知

    -d  --outputdir dir       Where to create output files. The default is the directory where tests are run from and the given path   is considered relative to that unless it is absolute.即指定生成报告的文件
    -L --loglevel level      Threshold level for logging. Available levels: TRACE,DEBUG, INFO (default), WARN, NONE (no logging).
     -i --include tag * 
     -v --variable name:value *      Set variables in the test data. Only scalar variables with string value are supported and name is given without `${}`.
      -R --rerunfailed output       Select failed tests from an earlier output file


##### rebot命令

使用：合并报告

    /usr/local/python3/bin/rebot -d Output --output output.xml --merge Output/output.xml Output/rerun.xml
    
   使用rebot --help可知

    -o --output file         XML output file. Not created unless this option is specified. Given path, similarly as paths given to --log, --report and --xunit, is relative to --outputdir unless given as an absolute path.
    -R --merge               When combining results, merge outputs together Example: rebot --merge orig.xml rerun.xml
    
  最终根据xml文件生成Log和Report文件（个人理解）
  
    Output:  /var/lib/jenkins/workspace/Staging/login for th/Output/output.xml
    Log:     /var/lib/jenkins/workspace/Staging/login for th/Output/log.html
    Report:  /var/lib/jenkins/workspace/Staging/login for th/Output/report.html
    
    