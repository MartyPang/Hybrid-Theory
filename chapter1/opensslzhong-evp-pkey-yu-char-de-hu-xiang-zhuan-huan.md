# OpenSSL中EVP\_PKEY与char\*相互转换

最近写的项目中使用了openssl椭圆曲线的签名与验签，中间需要实现EVP\_PKEY与char\*的相互转换以实现网络传输，查阅资料发现可以使用BIO类，具体实现如下。

## EVP\_PKEY --&gt; char\*

直接上代码。

```c++
int pkeyToChar(EVP_PKEY *pkey, char *cpkey)
{
  char *char_pkey = new char[512];
  BIO *outbio = BIO\_new(BIO_s_mem());

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

## char\* --> EVP\_PKEY
同样上代码。



```
int charToPkey(char *cpkey, EVP_PKEY *pkey)
{
  BIO *bio = NULL;
  
  if(NULL == (bio = BIO_new_mem_buf(cpkey, -1)))
  {
    cout<<"BIO new mem buf error."<<endl;
  }
  else if(NULL == (pkey = PEM_read_bio_PUBKEY(bio, NULL, NULL, NULL)))
  {
    ERR_load_crypto_strings();
    char errBuf[512];
    ERR_error_string_n(ERR_get_error(), errBuf, sizeof(errBuf));
    cout<< "load public key failed["<<errBuf<<"]"<<endl;
  }
  BIO_free_all(bio);
  return 0;
}
```




