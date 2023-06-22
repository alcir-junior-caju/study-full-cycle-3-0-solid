# Curso Full Cycle 3.0 - Módulo Solid

<div>
    <img alt="Criado por Alcir Junior [Caju]" src="https://img.shields.io/badge/criado%20por-Alcir Junior [Caju]-%23f08700">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-%23f08700">
</div>

---

## Descrição

O Curso Full Cycle é uma formação completa para fazer com que pessoas desenvolvedoras sejam capazes de trabalhar em projetos expressivos sendo capazes de desenvolver aplicações de grande porte utilizando de boas práticas de desenvolvimento.

---

## Repositório Pai
https://github.com/alcir-junior-caju/study-full-cycle-3-0

---

## Visualizar o projeto na IDE:

Para quem quiser visualizar o projeto na IDE clique no teclado a tecla `ponto`, esse recurso do GitHub é bem bacana

---
### SOLID
- SOLID é um acrônimo que consolida 5 itens que são considerados como boas práticas no mundo do desenvolvimento orientado a objetos.
    - SRP - Single Responsibility Principle;
    - OCP - Open-closed Principle;
    - LSP - Liskov Substitution Principle;
    - ISP - Interface Segregation Principle;
    - DIP - Dependency Inversion Principle;

#### Single Responsibility Principle
- Uma classe ela deve ter apenas uma responsabilidade, deve ter um e apenas um motivo para mudar;
- Exemplo de quebra desse princípio:

```php
    <?php

    class Course
    {
        private $name;
        private $category;
        private $description;

        public function connection ()
        {
            $pdo = new PDO();
            return $pdo;
        }

        public function createCategory()
        {
            $this->connection()->insert($this->category);
        }

        public function createCourse()
        {
            $this->connection()->insert($this->course);
        }

        public function getName()
        {
            return $this->name;
        }

        public function setName($name)
        {
            $this->name = $name;
            return $this;
        }

        public function getCategory()
        {
            return $this->category;
        }

        public function setCategory($category)
        {
            $this->category = $category;
            return $this;
        }
    }
```
- Como resolver com uma solução simples?
    - Essa classe deveria ser uma entidade anêmica de `Course` apenas com `getters` e `setters`.
    - Criar um outra entidade para `Category`;
    - E um `repository` para o banco de dados;

#### Open-closed Principle
- Toda classe deve ser aberta para extensão mas fechada para modificação.
- Exemplo de quebra desse princípio:

```php
    <?php

    class Video
    {
        private $type;

        public function calculateInterest()
        {
            if ($this->type === "Movie") {
                // calcula interesse baseado em filme
            } elseif ($this->type === "TVShow") {
                // calcula interesse baseado em serie
            }
        }
    }
```
- Como resolver com uma solução simples?
```php
    <?php

    abstract class Video
    {
        abstract public function calculateInterest();
    }

    class Movie extends Video
    {
        public function calculateInterest()
        {
            // calcula interesse baseado em filme
        }
    }

    class TVShow extends Video
    {
        public function calculateInterest()
        {
            // calcula interesse baseado em serie
        }
    }
```

#### Liskov Substitution Principle
- Subclasses podem ser substituidas por suas classes pai;
- Exemplo de quebra desse princípio:
```php
    <?php

    class Movie
    {
        public function play()
        {
            // play no video
        }

        public function increaseVolume()
        {
            // increase volume
        }
    }

    class TheLionKing extends Movie
    {

    }

    // Filme sem som
    class ModernTimes extends Movie
    {
        public function increaseVolume()
        {
            // deu ruim
        }
    }
```

#### Interface Segregation Principle
- Uma classe não é obrigada a implementar interfaces que ela não utilizará;
- Exemplo de quebra desse princípio:
```php
    <?php

    interface Movie
    {
        public function play();

        public function increaseVolume();
    }

    class TheLionKing implements Movie
    {
        public function play()
        {
            // play no video
        }

        public function increaseVolume()
        {
            // increase volume
        }
    }

    class ModernTimes implements Movie
    {
        public function play()
        {
            // play no video
        }

        public function increaseVolume()
        {
            // esse metodo não será utilizado, porém terá de ser implementado
        }
    }
```
- Como resolver com uma solução simples?
```php
    <?php

    interface Movie
    {
        public function play();
    }

    interface AudioControl
    {
        public function increaseVolume();
    }

    class TheLionKing implements Movie, AudioControl
    {
        public function play()
        {
            // play no video
        }

        public function increaseVolume()
        {
            // increase volume
        }
    }

    class ModernTimes implements Movie
    {
        public function play()
        {
            // play no video
        }
    }
```

#### Dependency Inversion Principle
- Dependa de abstrações e não de implemantações;
- Inverter as dependências;
- Exemplo de quebra desse princípio:
```php
    <?php

    class DramaCategory
    {

    }

    class Movie
    {
        private $name;
        private $category;

        public function getName()
        {
            return $this->name;
        }

        public function setName($name)
        {
            $this->name = $name;
        }

        public function getCategory()
        {
            return $this->category;
        }

        public function setCategory(DramaCategory $category)
        {
            $this->category = $category;
        }
    }
```
- Exemplo de quebra desse princípio:
```php
    <?php

    class DramaCategory
    {

    }

    class Movie
    {
        private $name;
        private $category;

        public function getName()
        {
            return $this->name;
        }

        public function setName($name)
        {
            $this->name = $name;
        }

        public function getCategory()
        {
            return new DramaCategory();
        }
    }
```
- Como resolver com uma solução simples?
```php
    <?php

   interface Category
   {

   }

   class DramaCategory implements Category
   {

   }

   class Movie
   {
        public function __construct($name, Category $category)
        {
            $this->name = $name;
            $this->category = $category;
        }

        private $name;
        private $category;

        public function getName()
        {
            return $this->name;
        }

        public function setName($name)
        {
            $this->name = $name;
        }

        public function getCategory()
        {
            return $this->category;
        }

        public function setCategory($category)
        {
            $this->category = $category;
        }
   }

   $movie = new Movie("name", new Category());
```
