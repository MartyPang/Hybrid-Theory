# OpenSSL中EVP_PKEY与char\*相互转换

最近写的项目中使用了openssl椭圆曲线的签名与验签，中间需要实现EVP_PKEY与char\*的相互转换以实现网络传输，查阅资料发现可以使用BIO类，具体实现如下。

## EVP_PKEY --> char*
直接上代码。


```c++
int pkeyToChar(EVP_PKEY *pkey, char *cpkey)
{
  char *char_pkey = new char[512];
  BIO *outbio = BIO\_new(BIO\_s\_mem());
  
  //write pubkey to BIO in PEM format
  if(!PEM\_write\_bio\_PUBKEY(outbio, pkey))
    BIO_printf(outbio, "Error writing public key data in PEM format");
  
  //get PEM data from mem bio
  BIO_get_mem_data(outbio, char_pkey);
  strcpy(cpkey, static_cast<const char*>(char_pkey);
  
  //free bio
  BIO_free_all(outbio);
  return 0;
}
```

