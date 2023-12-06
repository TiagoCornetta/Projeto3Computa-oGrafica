Projeto 3 - Computação Gráfica

Participantes:

Nome: Fernando Gabriel Chacon Fernandes Teruel do Prado RA:11201811700

Nome: Tiago Cornetta Campos RA:11201922123

WASM: https://cornettawebdesigner.github.io/Projeto3Computa-oGrafica/

DESCRIÇÃO DO PROJETO

O objetivo inicial era dar continuidade a atividade 2, porém houveram inumeros problemas, principalmente a ocorrência de bugs.
Em substituição a isso foi criado um visualizador 3D no qual diferentes objetos e texturas podem ser selecionadas. Adicionamos 2 objetos ao invés de apenas 1.

COMO FOI IMPLEMENTADO:

Utilizamos como base o exemplo viewer5 e fizemos algumas modificações modificações.
Todas as modificações foram feitas na classe window.cpp

```cpp
std::string atualizaTexturaNormal = "maps/brick_normal.jpg";
std::string atualizaTexturaDifuse = "maps/brick_base.jpg";

std::string objetoSelecionado_ = "cube.obj";
```

Criamos um método atualiza para atualizar o objeto a ser renderizado.

```cpp
void Window:: atualiza(){
  auto const assetsPath{abcg::Application::getAssetsPath()};
  // Load default model
  loadModel(assetsPath + objetoSelecionado);
  m_mappingMode = 3; // "From mesh" option

  // Initial trackball spin
  m_trackBallModel.setAxis(glm::normalize(glm::vec3(1, 1, 1)));
  m_trackBallModel.setVelocity(0.1f);
}
```

Para selecionar o objeto e a textura foi utilizado um combobox:
```cpp
 // Textura
    {
      static std::size_t currentIndex{};
      std::vector<std::string> comboItems{"brick", "pattern",
                                          "roman"};

      ImGui::PushItemWidth(120);
      if (ImGui::BeginCombo("Textura",
                            comboItems.at(currentIndex).c_str())) {
      for (auto const index : iter::range(comboItems.size())) {
        auto const isSelected{currentIndex == index};
        if (ImGui::Selectable(comboItems.at(index).c_str(), isSelected)) {
            currentIndex = index;
            if (index == 0 ) {
               atualizaTexturaNormal = "maps/brick_normal.jpg";
               atualizaTexturaDifuse = "maps/brick_base.jpg"   ;
            } else if (index == 1){
                atualizaTexturaNormal = "maps/pattern_normal.png";
               atualizaTexturaDifuse = "maps/pattern.png"   ;
            }
            else  {
                atualizaTexturaNormal = "maps/roman_lamp_normal.jpg";
               atualizaTexturaDifuse =  "maps/roman_lamp_diffuse.jpg" ;
            }
        }
    }

      ImGui::EndCombo();
    }
      ImGui::PopItemWidth();
    }

    // Object Box
    {
      static std::size_t currentIndex{};
      std::vector<std::string> comboItems{"Cube", "DeadTree",
                                          "BaseSniper"};
      ImGui::PushItemWidth(120);
     if (ImGui::BeginCombo("Object", 
                          comboItems.at(currentIndex).c_str())) {
      for (auto const index : iter::range(comboItems.size())) {
        auto const isSelected{currentIndex == index};

        if (ImGui::Selectable(comboItems.at(index).c_str(), isSelected)) {
            currentIndex = index;
            if (index == 0 ) {
                objetoSelecionado = "cube.obj";
            } else if (index == 1){
                objetoSelecionado = "DeadTree.obj";
            }
            else  {
                objetoSelecionado = "Sniper_Stand.obj";
            }
        }
      }

      ImGui::EndCombo();
    }
      ImGui::PopItemWidth();
    }
```

