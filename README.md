# poietic-freedom-repo

# Libertad Poiética: Modelo Free* para Agencia Instituyente  
> Repositorio complementario al artículo:  
> **"Libertad creadora: novedad, no-necesidad y autodeterminación sin regla previa"**  

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXX)  
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)  

## Contenidos  
- **Artículo completo**: Versión preprint con DOI ([PDF](/article/preprint.pdf))  
- **Kit de diagnóstico FDP**: Implementación del índice de libertad poiética (L*)  
- **Casos de estudio**: Datos estructurados para *habeas corpus* y porcelana Ming  
- **Bibliografía**: 25 referencias con DOI en BibTeX  

## Instalación Rápida  
```bash  
git clone https://github.com/tu-usuario/poietic-freedom-repo  
cd poietic-freedom-repo/fdp_toolkit  
pip install -r requirements.txt  
python fdp_calculator.py --case habeas_corpus.json

@misc{FernandezTamames2025,  
  author = {Fernández Tamames, José},  
  title  = {Libertad creadora: novedad, no-necesidad y autodeterminación sin regla previa},  
  year   = {2025},  
  doi    = {10.5281/zenodo.XXXXXX},  
  url    = {https://github.com/tu-usuario/poietic-freedom-repo}  
}

---

#### 2. **`fdp_toolkit/fdp_calculator.py`** (Herramienta Principal)  
```python
import json

# Pesos predefinidos para L*
WEIGHTS = {
    "ontological_novelty": 0.30,
    "non_deducibility": 0.25,
    "non_necessity": 0.20,
    "intentional_form": 0.15,
    "guidance_control": 0.10
}

def calculate_l_star(case_data):
    """
    Calcula el índice de libertad poiética (L*) para un caso dado
    """
    try:
        score = 0
        for criterion, weight in WEIGHTS.items():
            # Verifica cumplimiento binario (1/0) o parcial (0.0-1.0)
            criterion_value = case_data.get(criterion, 0)
            if isinstance(criterion_value, bool):
                criterion_score = 1.0 if criterion_value else 0.0
            else:
                criterion_score = float(criterion_value)
                
            score += weight * criterion_score
            
        return round(score, 3)
    
    except Exception as e:
        print(f"Error: {str(e)}")
        return None

if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser(description='Calculador del Índice de Libertad Poiética (L*)')
    parser.add_argument('--case', type=str, required=True, help='Archivo JSON con datos del caso')
    args = parser.parse_args()

    with open(args.case, 'r') as f:
        case_data = json.load(f)
    
    l_star = calculate_l_star(case_data)
    print(f"\n[RESULTADO] L* = {l_star} - Interpretación: ", end="")
    
    if l_star >= 0.9:
        print("Libertad Poiética Paradigmática (ej: habeas corpus)")
    elif l_star >= 0.7:
        print("Alta Libertad Creadora (ej: obra artística original)")
    elif l_star >= 0.5:
        print("Libertad Parcial (elementos creativos limitados)")
    else:
        print("No satisface criterios Free* (ej: salida algorítmica)")
