# DetalhaProduto.PHP
Criação do detalhaproduto.php

<?php
include('segurancadez.php');
include('cabecalho.php');
include('conn.php');
if(!isset($_GET['id'])){
    header('Location: listaprodutos.php');
    exit();
}
$id = $_GET['id'];
$sql = "SELECT * FROM tb_produtos WHERE id_produto = $id";
$result = mysqli_query($link,$sql);
$tbl = mysqli_fetch_array($result);
mysqli_close($link);
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="cadastra.css">
    <title>Cadastra Produto</title>
</head>
<body>
    <div class="container">
    <h1>Detalha Produto</h1>
    <br>
    <form action="detalhaproduto.php" method="post">
        <label for="nome">Nome:</label>
        <input type="text" value="<?=$tbl[1]?>" disabled>
        <br>
        <label for="descricao">Descrição:</label>
        <input type="text" value="<?=$tbl[2]?>" disabled>
        <br>
        <label for="caracteristica">Caracteristica:</label>
        <input type="text" value="<?=$tbl[3]?>" disabled>
        <br>
        <label for="estoque">Estoque:</label>
        <input type="number" value="<?=$tbl[4]?>" disabled>
        <br>
        <label for="valor">Valor:</label>
        <input type="number" value="<?=$tbl[5]?>" disabled>
        <br>
        <label for="imagem">Imagem:</label>
        <input type="imagens/" <?=$tbl[6]?>" disabled>
        <br>
        <label for="barcode">Codigo de Barras:</label>
        <input type="number" value="<?=$tbl[7]?>" disabled>
        <br>
        <br>
        <a href="listaprodutos.php"><input type="button" value="voltar">
        </a>
     </div>
    </form>
</body>
</html>
