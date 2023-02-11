# install php on mac (unsuported php like php 7.4)
```
brew tap shivammathur/php
brew install shivammathur/php/php@7.3
brew link php@7.3

brew services start php@7.4
```


# add ioncube 
```
#add on php.ini
zend_extension=/path/ioncube_der_7.4.so
```

