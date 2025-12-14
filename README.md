## Lab 2 : Conception du schéma et contraintes d’intégrité

## Script SQL
```
-- MariaDB dump 10.19  Distrib 10.4.32-MariaDB, for Win64 (AMD64)
--
-- Host: localhost    Database: bibliotheque
-- ------------------------------------------------------
-- Server version	10.4.32-MariaDB

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
USE bibliotheque;
--
-- Table structure for table `abonne`
--

DROP TABLE IF EXISTS `abonne`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `abonne` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `nom` varchar(100) NOT NULL,
  `email` varchar(150) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `email` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Table structure for table `auteur`
--

DROP TABLE IF EXISTS `auteur`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `auteur` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `nom` varchar(100) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Table structure for table `emprunt`
--

DROP TABLE IF EXISTS `emprunt`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `emprunt` (
  `ouvrage_id` int(11) NOT NULL,
  `abonne_id` int(11) NOT NULL,
  `date_debut` date NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `date_fin` date DEFAULT NULL,
  PRIMARY KEY (`ouvrage_id`,`abonne_id`,`date_debut`),
  KEY `fk_emprunt_abonne` (`abonne_id`),
  KEY `date_fin` (`date_fin`),
  CONSTRAINT `fk_emprunt_abonne` FOREIGN KEY (`abonne_id`) REFERENCES `abonne` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_emprunt_ouvrage` FOREIGN KEY (`ouvrage_id`) REFERENCES `ouvrage` (`id`),
  CONSTRAINT `ck_date_emprunt` CHECK (`date_fin` is null or `date_fin` >= `date_debut`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Table structure for table `ouvrage`
--

DROP TABLE IF EXISTS `ouvrage`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `ouvrage` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `titre` varchar(200) NOT NULL,
  `disponible` tinyint(1) NOT NULL DEFAULT 1,
  `auteur_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_ouvrage_auteur` (`auteur_id`),
  KEY `disponible` (`disponible`),
  CONSTRAINT `fk_ouvrage_auteur` FOREIGN KEY (`auteur_id`) REFERENCES `auteur` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
/*!40101 SET character_set_client = @saved_cs_client */;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2025-12-14 15:21:46





```


## Diagramme ER
![image alt](https://github.com/laouysalma/Tp2MySQL/blob/main/Diagramme.jpg?raw=true)

## Rapport

La base bibliotheque garantit des données fiables. Les clés primaires assurent l’unicité, les clés étrangères maintiennent l’intégrité entre auteurs, ouvrages et emprunts, avec ON DELETE/UPDATE CASCADE pour la cohérence automatique. La contrainte sur date_fin empêche les emprunts avec une date de retour avant le début. Les index sur disponible et date_fin accélèrent les recherches. Les colonnes NOT NULL et UNIQUE évitent doublons et informations manquantes. Cette conception rend la base stable, cohérente et facile à maintenir.
