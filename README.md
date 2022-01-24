# Starting-out-unity

## Frogger
https://user-images.githubusercontent.com/85858695/148811516-f6eed2f8-1ee5-4a16-a3f5-4a71efba7ef9.mp4






### Code Snippets

Move down in arcade style

```C#
transform.Translate(0, -intensity * speed, 0);
```
## Cut the rope
https://user-images.githubusercontent.com/85858695/148811530-550be6e6-3d0a-46e2-aa89-07048c8b820c.mp4



### Code Snippets


#### Form the rope 
- Keep connecting the mini-links to their previous link
- Except for the last one which should be connected to the candy (weight).
```C#
 void Rope()
    {
        Rigidbody2D previousRB = hinge;
        for(int i = 0; i < size; i++)
        {
            GameObject link = Instantiate(linkPrefab, transform);
            HingeJoint2D joint = link.GetComponent<HingeJoint2D>();
            joint.connectedBody = previousRB;
            if (i == size - 1)
            {
                Weight(link.GetComponent<Rigidbody2D>()); 
            }
            else
            {
                previousRB = link.GetComponent<Rigidbody2D>();
            }
        }
    }
```
#### Candy (weight) 
- Check the details required in the inspector for the hinge joint and add data accordingly through the script 
- Call this function while connecting the last link to the candy
```C#
    void Weight(Rigidbody2D endRB)
    {
        HingeJoint2D weight = GameObject.FindGameObjectWithTag("Weight").AddComponent<HingeJoint2D>();
        weight.autoConfigureConnectedAnchor = false;
        weight.anchor = Vector2.zero;
        weight.connectedBody = endRB;
        weight.connectedAnchor = new Vector2(0, -distanceFromChainEnd);
    }
```
## Color switch
https://user-images.githubusercontent.com/85858695/148812220-7472ba4f-8f86-49cf-a9ac-42aba03004c6.mp4



## AA
https://user-images.githubusercontent.com/85858695/148813946-16b4a867-d58a-4c1f-99d6-59d391d68ec1.mp4



## Basic shooting
https://user-images.githubusercontent.com/85858695/148815664-250a4673-4b09-4bb5-8280-3095160255f8.mp4



## Escalator
https://user-images.githubusercontent.com/85858695/148816077-438462aa-91c3-4f7d-818c-3576d252bfc1.mp4



## Endless runner
https://user-images.githubusercontent.com/85858695/148812892-f007dc7f-b69d-45eb-9f67-6d358986f182.mp4

