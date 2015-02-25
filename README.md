# TP_GameOfThrones-arangoDB
TP arangoDB avec les personnages de Game Of Thrones. La base de données de personnages s'appelle : `gameOfThrone`. Celle de liens de parenté s'appelle : `bond`.

Commande pour démarrer arango db : `/usr/local/sbin/arangod`

Accès au client : `http://localhost:8529/`

Quelques requetes :

* Tous les personnages masculins : `FOR character IN gameOfThrone
  FILTER character.gender == "m"
  RETURN character`.

* Tous les enfants (fils et/ou fille). Tous les enfants de Eddard Start : `FOR oneBond IN bond
  FILTER oneBond._from == "gameOfThrone/Eddard_Stark"
  FOR charater in gameOfThrone
    FILTER CONCAT("gameOfThrone/",charater._key) == oneBond._to/* && charater.gender == "f"*/
    RETURN charater.name`.

* Tous les parents (pere et/ou mere). Les parents de Sansa Stark : `FOR oneBond IN bond
  FILTER oneBond._to == "gameOfThrone/Sansa_Stark"
  FOR charater in gameOfThrone
    FILTER CONCAT("gameOfThrone/",charater._key) == oneBond._from/* && charater.gender == "f"*/
    RETURN charater.name`

* Tous les grand-parents (grand-pere et/ou grand-mere). Les grands-peres de Sansa Stark : `FOR oneBond IN bond
  FILTER oneBond._to == "gameOfThrone/Sansa_Stark"
  FOR charater in gameOfThrone
    FILTER CONCAT("gameOfThrone/",charater._key) == oneBond._from && charater.gender == "m"
    FOR oneBond2 IN bond
      FILTER oneBond2._to == CONCAT("gameOfThrone/",charater._key)
      FOR charater2 in gameOfThrone
        FILTER CONCAT("gameOfThrone/",charater2._key) == oneBond2._from && charater2.gender == "m"
        RETURN charater2.name`

* Tous les oncles et tantes. Les oncles de Sansa Stark : `FOR oneBond IN bond
  FILTER oneBond._to == "gameOfThrone/Sansa_Stark"
  FOR charater in gameOfThrone
    FILTER CONCAT("gameOfThrone/",charater._key) == oneBond._from && charater.gender == "m"
    FOR oneBond2 IN bond
      FILTER oneBond2._to == CONCAT("gameOfThrone/",charater._key)
      FOR charater2 in gameOfThrone
        FILTER CONCAT("gameOfThrone/",charater2._key) == oneBond2._from && charater2.gender == "m"
        FOR oneBond3 IN bond
          FILTER oneBond3._from == CONCAT("gameOfThrone/",charater2._key)
          FOR charater3 in gameOfThrone
            FILTER CONCAT("gameOfThrone/",charater3._key) == oneBond3._to && charater3._key != charater._key
            RETURN charater3.name`

