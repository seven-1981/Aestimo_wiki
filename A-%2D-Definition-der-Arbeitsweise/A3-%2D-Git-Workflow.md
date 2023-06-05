Die folgende Abbildung zeigt den zu verwendenden Git Workflow. Der Masterbranch muss dabei immer in einem lauffähgen Zustand sein. Vom Masterbranch wird abgezweigt in einen Developbranch. Für die Feature-Implementierung wird vom Develop abgebrancht und über einen Pull Request zurück in den Develop gemerged.

Sobald ein zu versionierender Stand erreicht ist, wird der Develop zurück in den Master gemerged. Auf dem Master wird dann ein Tag erstellt. 

![image.png](/.attachments/image-aeee0745-0b9a-40b3-82ad-90a2352346ec.png)
Quelle: https://www.bluesource.at/blog/detail/git-flow