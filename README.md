------

Gitlab runner executor : docker

------
Deploy Only Hosts :
- Ana sizinde bulunan .gitlab-ci dosyasının içerisindeki include keyword 'ünde dev klasörümüzün içerisindeki .gitlab-ci.yml isimli dosyayı seçmemiz yeterli. Bu aşamada sadece build jar 'ımızı oluşturup sunucumuza copyalayalanır. Sunucular Update.sh gibi bir script ile (Ansible da kullanılabilir.) istenildiği gibi güncellenebilir.

Deploy Docker Hosts :
- Docker image ları kullanmak istiyorsak. Ana sizinde bulunan .gitlab-ci dosyasının içerisindeki include keyword 'ünde dev klasörümüzün içerisindeki .gitlab-ci-with-docker.yml isimli dosyayı seçmemiz yeterli. Sunucular Update.sh gibi bir script ile (Ansible da kullanılabilir.) istenildiği gibi güncellenebilir.

------

Docker Repository için lazım olan değişkenler : 

 - HARBOR_REGISTRY_USER  : Harbor_user eklenecek veya projemizin özelinde oluşturduğumuz harbor robotunun bilgileri eklenecek.

 - HARBOR_REGISTRY_USER_PASSWORD :  Harbor_user ‘ın passwordu eklenecek veya oluşturduğumuz robot’un token ı eklenecek.

 - HARBOR_REGISTRY  : Private harbor ‘umuzun erişim URL bilgisini ekleyeceğiz.

 - HARBOR_REGISTRY_TAG : URL 'in sade hali

 - HARBOR_NAMESPACE : Harbor un içerisinde projemize özel oluşturduğumuz repository nin ismini ekliyoruz.

  Not : Bu değişkenlerin içeriği sizin kullanmış olduğunuz docker repository nize göre değişir.

---
Deploy işlemlerini gerçekleştireceğimiz sunucumuz için lazım olan değişken ve SSH key oluşturma :

 - DEPLOY_HOST_SSH_PRIVATE_KEY : id_ed25519 dosyasının içerisini ekleriz. 

  Gitlab runnerın docker executor da çalışması için ed25519 formatını destekler.

   - ssh-keygen -t ed25519  → SSH key oluşturur  /home/.ssh/ id_ed25519 ve id_ed25519.pub dosyalarını oluşturur.

   - cat id_ed25519.pub >> ~/.ssh/authorized_keys → id_ed25519.pub dosyalarını dosyasındaki bilgileri authorized_keys’in içerisine atıyoruz.

   - id_ed25519 -> Gitlab ‘a ekleyeceğimiz $DEPLOY_HOST_SSH_PRIVATE_KEY değişkenine. Bağlantı yapmak istediğimiz sunucuda bulunan kullanıcıya ait  /home/.ssh/     id_ed25519 dosyasının içerisini ekleriz. 

  Not : Eğer erişim sorunu yaşarsak aşağıdaki işlemleri de yapabiliriz

   - sudo nano /etc/ssh/sshd_config → Kapalı olan PubkeyAuthentication yes özelliğini açıyoruz.

   - sudo /etc/init.d/ssh force-reload → Yukarıdaki değişikliği yaparsak servisi yeniden başlatmamız gerekiyor.

   - sudo /etc/init.d/ssh restart
---
