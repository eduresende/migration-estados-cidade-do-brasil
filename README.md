Migration que popula o banco de dados com os estados e cidades do Brasil com dados extraídos do site do IBGE, em http://www.sidra.ibge.gov.br/bda/territorio/download/dtb2006.zip

Para utilizá-la é necessário criar os seguintes models:

##Rails 3##

`Estado.rb`
```
class Estado < ActiveRecord::Base
  attr_accessible :nome, :sigla

  has_many :cidades

  belongs_to :capital, :class_name => 'Cidade'
end
```

`Cidade.rb`
```
class Cidade < ActiveRecord::Base
  belongs_to :estado
  attr_accessible :nome
end
```

##Rails 4##

`Estado.rb`
```
class Estado < ActiveRecord::Base
  has_many :cidades

  belongs_to :capital, :class_name => 'Cidade'

  def estado_params
    params.require(:estado).permit(:nome, :sigla, :capital)
  end
end
```
`Cidade.rb`
```
class Cidade < ActiveRecord::Base
  belongs_to :estado

  def cidade_params
    params.require(:cidade).permit(:nome)
  end
end
```

- Modificado o migration para que funcione com versões do Rails 2, 3 e 4
- Incluindo '# -*- coding: UTF-8 -*-' no topo do arquivo para ruby 1.9.x
- Modifiquei o nome da classe da migration que popula os estados e cidades. O rake esta comparando o nome do arquivo com o nome da classe e gerando um erro.
- Incluí a propriedade "capital" no model "Estado", para que seja possivel identificar a cidade que é capital do estado, para ordenar um combo priorizando a capital, por exemplo.