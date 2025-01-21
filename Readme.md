import boto3

def recognize_celebrities(image_bytes):
    # Crie um cliente para o serviço Amazon Rekognition
    rekognition = boto3.client('rekognition')
    
    # Chame a operação RecognizeCelebrities
    response = rekognition.recognize_celebrities(
        Image={
            'Bytes': image_bytes
        }
    )
    
    # Processa a resposta
    celebrity_faces = response.get('CelebrityFaces', [])
    unrecognized_faces = response.get('UnrecognizedFaces', [])

    return celebrity_faces, unrecognized_faces

# Exemplo de uso
if __name__ == "__main__":
    # Abra a imagem em formato binário
    with open('caminho_para_sua_imagem.jpg', 'rb') as image_file:
        image_bytes = image_file.read()
    
    # Chama a função de reconhecimento de celebridades
    celebrity_faces, unrecognized_faces = recognize_celebrities(image_bytes)
    
    # Exibe os resultados
    print("Celebridades reconhecidas:")
    for celebrity in celebrity_faces:
        print(f"Nome: {celebrity['Name']}")
        print(f"ID: {celebrity['Id']}")
        print(f"Confiança: {celebrity['MatchConfidence']}")
        print(f"Mais informações: {celebrity['Urls']}")
        print()

    print("Rostos não reconhecidos:")
    for face in unrecognized_faces:
        print(f"Confiança: {face['Confidence']}")
        print(f"BoundingBox: {face['BoundingBox']}")
        print()
