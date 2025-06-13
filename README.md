# DetalhaProduto.PHP
Criação do detalhaproduto.php

<?php
// Inclui arquivos de segurança, cabeçalho da página e conexão com o banco de dados //
include('segurancadez.php');
include('cabecalho.php');
include('conn.php');

// Verifica se o parâmetro 'id' foi passado via GET //
if(!isset($_GET['id'])){
    // Se não houver ID, redireciona para a lista de produtos //
    header('Location: listaprodutos.php');
    exit();
}

// Obtém o ID do produto a partir da URL //
$id = $_GET['id'];

// Consulta todos os dados do produto com o ID informado //
$sql = "SELECT * FROM tb_produtos WHERE id_produto = $id";
$result = mysqli_query($link, $sql);
$tbl = mysqli_fetch_array($result); // Armazena os dados do produto em um array //

// Se o produto não for encontrado, redireciona para a página inicial //
if(!$tbl){
    mysqli_close($link);
    header('Location: index.php');
    exit();
}
// Fecha a conexão com o banco de dados //
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
        <input type="imagens" value="<?=$tbl[6]?>" disabled>
        <br>
        <label for="barcode">Codigo de Barras:</label>
        <input type="number" value="<?=$tbl[7]?>" disabled>
        <br>
        <br>
        <a href="listaprodutos.php"><input type="button" value="voltar">
        </a>
     </div>
    </form>
    <!-- Imagem e informações principais -->
    <img src="imagens/<?= $tbl['imagem_produto'] ?>" height="250">
        <span class="titulo"><?= $tbl['nome_produto'] ?></span>
        <span class="product-price">
            R$ <?= number_format($tbl['valor_produto'], 2, ',', '.') ?>
        </span>
        <p>Estoque disponível: <?= $tbl['estoque_produto'] ?> unidades</p>

        <!-- Formulário para adicionar ao carrinho -->
<form action="carrinho.php" method="post">
            <input type="hidden" name="id_produto" value="<?= $tbl['id_produto'] ?>">
            <select name="quantidade">
                <?php for($i = 1; $i <= 10; $i++): ?>
                    <option value="<?= $i ?>"><?= $i ?></option>
                <?php endfor; ?>
            </select>
            <br>
            <a href="index.php">
                <input type="button" value="Voltar">
            </a>
            <input type="submit" value="Adicionar ao Carrinho">
        </form>

        <!-- Descrição e características -->
<p>Descrição: <?= $tbl['descricao_produto'] ?></p>
        <p>Características: <?= $tbl['caracteristica_produto'] ?></p>
        <p>Código de Barras: <?= $tbl['barcode_produto'] ?></p>

        <!-- Produtos relacionados -->
<h2>Produtos Relacionados</h2>
        <ul>
        <?php
        // Consulta produtos relacionados (exemplo simples) //
        $sql = "SELECT id_produto, nome_produto FROM tb_produtos 
                WHERE id_produto != $id AND status_produto = 1 
                ORDER BY RAND() LIMIT 3";
        $result_rel = mysqli_query($link, $sql);
        while($rel = mysqli_fetch_array($result_rel)){
            echo "<li><a href='detalhaproduto.php?id={$rel['id_produto']}'>{$rel['nome_produto']}</a></li>";
        }
        mysqli_close($link);
        ?>
        </ul>
        </div>
</body>
</html>
