# Compte rendu : TP API LogiAI

**Nom :** Ahmed Masbah  **Groupe :** G4

## 1. Environnement et premiers tests
- Sortie de `python -m pytest` au départ (collez-la) :

(.venv) PS C:\Users\HP\Desktop\starter> python -m pytest                         
============================================================= test session starts ==============================================================
platform win32 -- Python 3.13.3, pytest-9.2.0, pluggy-1.7.0
rootdir: C:\Users\HP\Desktop\starter
plugins: anyio-4.15.1
collected 13 items                                                                                                                              

tests\test_api.py ..FFFF......F                                                                                                           [100%]

=================================================================== FAILURES ===================================================================
_______________________________________________________ test_predict_valide_renvoie_200 ________________________________________________________

    def test_predict_valide_renvoie_200():
        with TestClient(app) as client:
>           assert post(client).status_code == 200
E           assert 422 == 200
E            +  where 422 = <Response [422 Unprocessable Entity]>.status_code
E            +    where <Response [422 Unprocessable Entity]> = post(<starlette.testclient.TestClient object at 0x000002A26B0A5090>)

tests\test_api.py:30: AssertionError
_______________________________________________________ test_predict_forme_de_la_reponse _______________________________________________________

    def test_predict_forme_de_la_reponse():
        with TestClient(app) as client:
            data = post(client).json()
>           assert set(data) == {"probabilite_retard", "alerte", "seuil", "model_version"}
E           AssertionError: assert {'detail'} == {'alerte', 'm...ard', 'seuil'}
E             
E             Extra items in the left set:
E             'detail'
E             Extra items in the right set:
E             'probabilite_retard'
E             'seuil'
E             'model_version'
E             'alerte'
E             Use -v to get more diff

tests\test_api.py:36: AssertionError
____________________________________________________ test_predict_probabilite_entre_0_et_1 _____________________________________________________

    def test_predict_probabilite_entre_0_et_1():
        with TestClient(app) as client:
>           assert 0 <= post(client).json()["probabilite_retard"] <= 1
                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E           KeyError: 'probabilite_retard'

tests\test_api.py:41: KeyError
___________________________________________ test_predict_trajet_risque_plus_eleve_que_trajet_facile ____________________________________________
    facile = dict(distance_km=5, poids_kg=3, heure_depart=11, pluie_mm=0,
                  trafic_index=1, nb_arrets=0, type_vehicule="moto")
    with TestClient(app) as client:
>           p_difficile = post(client).json()["probabilite_retard"]
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E           KeyError: 'probabilite_retard'

tests\test_api.py:48: KeyError
________________________________________________________ test_modele_absent_renvoie_503 ________________________________________________________

    def test_modele_absent_renvoie_503():
        with TestClient(app) as client:
            state.pop("predictor", None)  # on simule un modèle non chargé
>           assert post(client).status_code == 503
E           assert 422 == 503
E            +  where 422 = <Response [422 Unprocessable Entity]>.status_code
E            +    where <Response [422 Unprocessable Entity]> = post(<starlette.testclient.TestClient object at 0x000002A26B01BAC0>)

tests\test_api.py:87: AssertionError
=============================================================== warnings summary ===============================================================
.venv\Lib\site-packages\fastapi\testclient.py:1
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\fastapi\testclient.py:1: StarletteDeprecationWarning: Using `httpx` with `starlette.testclient` is deprecated; install `httpx2` instead.
    from starlette.testclient import TestClient as TestClient  # noqa

tests/test_api.py: 13 warnings
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\sklearn\base.py:525: InconsistentVersionWarning: Trying to unpickle estimator StandardScaler from version 1.7.2 when using version 1.9.1. This might lead to breaking code or invalid results. Use at your own risk. For more info please refer to:
  https://scikit-learn.org/stable/model_persistence.html#security-maintainability-limitations
    warnings.warn(

tests/test_api.py: 13 warnings
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\sklearn\base.py:525: InconsistentVersionWarning: Trying to unpickle estimator OneHotEncoder from version 1.7.2 when using version 1.9.1. This might lead to breaking code or invalid results. Use at your own risk. For more info please refer to:
  https://scikit-learn.org/stable/model_persistence.html#security-maintainability-limitations
    warnings.warn(

tests/test_api.py: 13 warnings
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\sklearn\base.py:525: InconsistentVersionWarning: Trying to unpickle estimator ColumnTransformer from version 1.7.2 when using version 1.9.1. This might lead to breaking code or invalid results. Use at your own risk. For more info please refer to:
  https://scikit-learn.org/stable/model_persistence.html#security-maintainability-limitations
    warnings.warn(

tests/test_api.py: 13 warnings
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\sklearn\base.py:525: InconsistentVersionWarning: Trying to unpickle estimator LogisticRegression from version 1.7.2 when using version 1.9.1. This might lead to breaking code or invalid results. Use at your own risk. For more info please refer to:
  https://scikit-learn.org/stable/model_persistence.html#security-maintainability-limitations
    warnings.warn(

tests/test_api.py: 13 warnings
  C:\Users\HP\Desktop\starter\.venv\Lib\site-packages\sklearn\base.py:525: InconsistentVersionWarning: Trying to unpickle estimator Pipeline from version 1.7.2 when using version 1.9.1. This might lead to breaking code or invalid results. Use at your own risk. For more info please refer to:
  https://scikit-learn.org/stable/model_persistence.html#security-maintainability-limitations
    warnings.warn(

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
=========================================================== short test summary info ============================================================
FAILED tests/test_api.py::test_predict_valide_renvoie_200 - assert 422 == 200
FAILED tests/test_api.py::test_predict_forme_de_la_reponse - AssertionError: assert {'detail'} == {'alerte', 'm...ard', 'seuil'}
FAILED tests/test_api.py::test_predict_probabilite_entre_0_et_1 - KeyError: 'probabilite_retard'
FAILED tests/test_api.py::test_predict_trajet_risque_plus_eleve_que_trajet_facile - KeyError: 'probabilite_retard'
FAILED tests/test_api.py::test_modele_absent_renvoie_503 - assert 422 == 503
================================================== 5 failed, 8 passed, 66 warnings in 40.05s ===================================================
(.venv) PS C:\Users\HP\Desktop\starter> 



- Pourquoi certains tests qui attendent un 422 passent-ils déjà, alors que rien n'est écrit ?

Au début, les tests attendant un code 422 passaient pour la mauvaise raison : le schéma Pydantic étant vide, il rejetait tous les champs inconnus (y compris les données valides). C'est le test de succès attendant un code 200 qui a démasqué ce "faux vert" en échouant, prouvant ainsi que le schéma devait être complété pour valider correctement les bornes plutôt que de simplement rejeter la requête.


## 2. Mon schéma
- Mes bornes et leur justification physique (une ligne par variable) :

| Variable | Min | Max | Pourquoi |
|---|---|---|---|
| distance_km | 1 | 1000 | Un trajet de livraison ne peut pas être négatif ou nul, et 1000 km est une limite réaliste pour une livraison nationale. |
| poids_kg | 1 | 3000 | Le poids doit être positif. 3000 kg correspond à la capacité maximale d'un gros camion de livraison. |
| heure_depart | 0 | 23 | Il n'y a que 24 heures dans une journée (de 0 à 23). |
| pluie_mm | 0 | 100 | La pluie ne peut pas être négative. Au-delà de 100 mm, c'est une catastrophe naturelle qui bloquerait toute livraison. |
| trafic_index | 0 | 10 | C'est l'échelle imposée par le contrat de l'API (0 = fluide, 10 = saturé). |
| nb_arrets | 0 | 20 | Une livraison peut avoir 0 arrêt, et 20 est une limite réaliste pour une tournée de livraison. |

## 3. Ma fonction predict()
```python
@app.post("/v1/predict", response_model=PredictionOut)
def predict(livraison: Livraison):
    # 1. Récupérer le predictor dans state
    predictor = state.get("predictor")
    
    # 2. S'il est absent, lever une erreur 503
    if predictor is None:
        raise HTTPException(
            status_code=503, 
            detail="Modèle non chargé. Lancez l'entraînement puis redémarrez le serveur."
        )
        
    # 3. Sinon, renvoyer la prédiction
    return PredictionOut(**predictor.predict(livraison.model_dump()))