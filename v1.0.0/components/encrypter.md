# Encrypter
Use `EncrypterInterface` to encrypt/decrypt data based on application key which is defined in `app/config/encrypter.php` and located in `.env` file.

> Key will be set automatically during application installation.

## EncrypterInterface
```php
interface EncrypterInterface
{
    /**
     * Create and encrypter instance with new key.
     *
     * @param string $key
     *
     * @return self
     * @throws EncrypterException
     */
    public function withKey(string $key): EncrypterInterface;

    /**
     * Encryption ket value. Returns in a format of ANSI string.
     *
     * @return string
     */
    public function getKey(): string;

    /**
     * Encrypt data into encrypter specific payload string. Can be decrypted only using decrypt()
     * method.
     *
     * @see decrypt()
     *
     * @param mixed $data
     *
     * @return string
     * @throws EncryptException
     * @throws EncrypterException
     */
    public function encrypt($data): string;

    /**
     * Decrypt payload string. Payload should be generated by same encrypter using encrypt() method.
     *
     * @see encrypt()
     *
     * @param string $payload
     *
     * @return mixed
     * @throws DecryptException
     * @throws EncrypterException
     */
    public function decrypt(string $payload);
}
```

Default implementation of encrypter based on 2nd version of [defuse/php-encryption](https://github.com/defuse/php-encryption).

Encrypter always available as injection or shortcut "encrypter":

```php
protected function indexAction(EncrypterInterface $encrypter)
{
    //Never expose encryption key to website users
    dump($encrypter->getKey());

    dump($payload = $encrypter->encrypt(['abc']));
    dump($this->encrypter->decrypt($payload));
}
```

> Run `app:key` to change your application key.