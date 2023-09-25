# censura-it

Avendo la necessità di gestire le blacklist per il mio ISP ho deciso di condividerle con tutti coloro di cui ne avessero bisogno.

Per le liste CNCPO, AAMS e tabacchi sono associati agli url le relative stop-page.
Tutte le altre liste puntano a 127.0.0.1 e dovete crearvi voi la stop-page.

❗❗ATTENZIONE❗❗

Io mi impegno al massimo per poter avere le liste quanto più aggiornate e corrette
TUTTAVIA potrebbero esserci errori o entry mancanti
RAGION PER CUI mi sollevo da OGNI responsabilità.

Se avete url da aggiungere o suggerimenti per migliorare il tutto vi prego di mandarmi una mail a nick11625@gmail.com

Baci🥰

**Installazione**
Per poter utilizzare le liste è necessario avere unbound installato.
Il modo che io trovo più comodo per realizzare un server DNS  è piHole.
Per installarlo basta seguire la guida che si trova a questo indirizzo 
https://docs.pi-hole.net/main/basic-install/

Per installare unbound e configurarlo per piHole segui questa guida 
https://docs.pi-hole.net/guides/dns/unbound/#setting-up-pi-hole-as-a-recursive-dns-server-solution

Se non ti piace leggere questo video fa al caso tuo
[Craft Computing: Setting up your own Recursive DNS Server!](https://www.youtube.com/watch?v=FnFtWsZ8IP0)

Una volta installati entrambi, installa unbound-blacklist seguendo la guida che trovi qui
https://github.com/alexsotoaguilera/unbound-blacklist/tree/master

Quando arrivi alla sezione di configurazione, utilizza la seguente
/etc/unbound-blacklist/conf.json

```
{
  "blocking_mode": "always_nxdomain",
  "unbound_conf_path": "/etc/unbound/unbound.conf.d/",
  "whitelist_path": "/etc/unbound-blacklist/whitelist",

  "blacklist":{
    "consob": {
      "enabled": true,
      "url": "https://raw.githubusercontent.com/nick11625/censura-it/main/hosts/consob",
      "input_format": "127.0.0.1 DOMAIN",
      "config": "/etc/unbound/unbound.conf.d/unbound-blacklist_consob.conf"
    },
    "tabacchi": {
      "enabled": true,
      "url": "https://raw.githubusercontent.com/nick11625/censura-it/main/hosts/tabacchi",
      "input_format": "217.175.53.72 DOMAIN",
      "config": "/etc/unbound/unbound.conf.d/unbound-blacklist_tabacchi.conf"
    },
    "aams": {
      "enabled": true,
      "url": "https://raw.githubusercontent.com/nick11625/censura-it/main/hosts/aams",
      "input_format": "217.175.53.72 DOMAIN",
      "config": "/etc/unbound/unbound.conf.d/unbound-blacklist_aams.conf"
    },
    "agcom": {
        "enabled": true,
        "url": "https://raw.githubusercontent.com/nick11625/censura-it/main/hosts/agcom",
        "input_format": "127.0.0.1 DOMAIN",
        "config": "/etc/unbound/unbound.conf.d/unbound-blacklist_agcom.conf"
      }
  }
}
```

Abilita il timer
```
sudo systemctl enable unbound-blacklist-updater.timer
```

Avvia il servizio ed il timer
```
sudo systemctl start unbound-blacklist-updater.service
sudo systemctl start unbound-blacklist-updater.timer
```

Ricostruisci le blacklist
```
sudo unbound-blacklist
```

Finito🥰