# PureDb
Json DB in PHP portable and easy 
```php
      $db = new PureDb("banco.json");
      $line['nome'] = "Victor";
      $line['sexo'] = "Masculino";
      $db->insert($line);

      $line['nome'] = "Felipe";
      $line['sexo'] = "Masculino";
      $line['idade'] = 28;
      $db->insert($line);

      for ($index = 0; $index < 10 * 1000; $index++) {
          $line['nome'] = "Teste";
          $line['sexo'] = "";
          $line['numero'] = $index;
          $lines[] = $line;
      }
      $db->insertArray($lines);


      $up['sexo'] = 'masculino';
      $db->update($up, 'like($nome,"f%")');

      $db->delete('$numero > 5');

      $r = $db->select('like($sexo,"mas%") || $nome == "Teste"');


      foreach ($r as $value) {

          echo "$value[id] $value[nome]<br>";
      }
```
