<?php

class PureDb {

    private $file;

    function like($campo, $valor) {
        $valor = str_replace("%", "*", $valor);
        return fnmatch(strtoupper($valor), strtoupper($campo));
    }

    function read() {
        $file = $this->file;
        if (!file_exists($file)) {
            $dados = array(
                array('id' => 0)
            );
            file_put_contents($file, json_encode($dados));
            $this->read();
        }
        $retorno = array();
        $arrayObject = json_decode(file_get_contents($file));
        foreach ($arrayObject as $array) {
            $retorno[] = (array) $array;
        }
        return $retorno;
    }

    function write($data) {
        $file = $this->file;
        $data = json_encode($data);
        if (file_put_contents($file, $data)) {
            return true;
        } else {
            return false;
        }
    }

    function error($error, $function) {
        ?>
        <h1 style="text-align:center;color:red"><?php echo $error ?> em <?php echo $function ?></h1>
        <?php
        return false;
    }

    function insertArray($lines) {
        if (count($lines) < 1) {
            return $this->error('Deve existir pelo menos uma linha', "insertArray($line)");
        }
        if (array_keys($lines[0]) < 1) {
            return $this->error('Sua linha deve ter chaves', "insertArray($line)");
        }
        $db = $this->read();
        $ids = array();
        foreach ($lines as $line) {
            $id = $db[count($db) - 1];
            $id = (array) $id;
            $id = $id['id'];
            $id++;
            $line['id'] = $id;
            if (count($db) > 0) {
                $db[] = $line;
            } else {
                $db = array($line);
            }
            $ids[] = $id;
        }
        if ($this->write($db)) {
            return $ids;
        }
    }

    function insert($line) {
        $file = $this->file;
        if (array_keys($line) < 1) {
            return $this->error('Sua linha deve ter chaves', "insert($line)");
        }
        $db = $this->read();
        $id = $db[count($db) - 1];
        $id = (array) $id;
        $id = $id['id'];
        $id++;
        $line['id'] = $id;
        if (count($db) > 0) {
            $db[] = $line;
        } else {
            $db = array($line);
        }
        if ($this->write($db)) {
            return $id;
        }
    }

    function select($where) {
        $file = $this->file;
        if ($where == '') {
            $new = $this->read();
            unset($new[0]);
            return $new;
        }
        $where = str_replace('like(', '$this->like(', $where);
        $new = array();
        $read = $this->read();
        foreach ($read as $d) {
            $d = (array) $d;
            foreach (array_keys($d) as $v) {
                eval('$' . $v . '= "' . $d[$v] . '";');
            }
            @eval('$b = (' . $where . ')?true:false;');
            if ($b) {
                $new[] = $d;
            }
        }
        return $new;
    }

    public function __construct($file) {
        $this->file = $file;
    }

    function delete($where) {
        $file = $this->file;
        $indexs = array();
        $new = array();
        $db = $this->read();
        if ($where == '') {
            $new = $db;
            for ($index = 0; $index < count($new); $index++) {
                $indexs[] = $index;
            }
        }
        $where = str_replace('like(', '$this->like(', $where);
        $contador = 0;
        foreach ($db as $d) {
            $d = (array) $d;
            foreach (array_keys($d) as $v) {
                eval('$' . $v . '= "' . $d[$v] . '";');
            }
            @eval('$b = (' . $where . ')?true:false;');
            if ($b) {
                $indexs[] = $contador;
            }
            $contador++;
        }
        foreach ($indexs as $i) {
            unset($db[$i]);
        }
        $this->write($db);
    }

    function update($line, $where) {
        $file = $this->file;
        $indexs = array();
        $new = array();
        $db = $this->read();
        if ($where == '') {
            $new = $db;
            for ($index = 0; $index < count($new); $index++) {
                $indexs[] = $index;
            }
        }
        $where = str_replace('like(', '$this->like(', $where);
        $contador = 0;
        foreach ($db as $d) {
            $d = (array) $d;
            foreach (array_keys($d) as $v) {
                eval('$' . $v . '= "' . $d[$v] . '";');
            }
            @eval('$b = (' . $where . ')?true:false;');
            if ($b) {
                $indexs[] = $contador;
            }
            $contador++;
        }
        foreach ($indexs as $i) {
            foreach (array_keys($line) as $chave) {
                $db[$i][$chave] = $line[$chave];
            }
        }
        $this->write($db);
    }

}
