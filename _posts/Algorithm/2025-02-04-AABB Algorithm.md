---
title:  "RB(RED-BLACK) TREE" 

categories:
  - Algorithm
tags:
  - [알고리즘, 충돌 체크, AABB]

toc: true
toc_sticky: true

date: 2025-02-04
last_modified_at: 2025-02-04
---

#AABB(Axis Aligned Bounding Box) 알고리즘


AABB 알고리즘은 충돌체크를 할 때 사용하는 알고리즘이다.

직각 좌표계의 각 축에 평행한 사각형 Collider에 대해서, 그 사각형들의 최소 / 최대 좌표값을 사용하여 충돌을 하는 알고리즘이다. 

![Image](https://github.com/user-attachments/assets/26e1d6b3-7fbb-43ce-b065-b61e24377c61)

단적으로 X축을 통한 그림으로 알아보자. 

X축에서 겹치는 케이스는 2가지가 있다고 볼 수 있다.

좌측에서 겹치는, 우측에서 겹치는 경우이고 위에서 나온 식을 만족한다면 X축으로 충돌했다고 생각하면 된다.


또, Y축을 추가로 넣어서 한 번 보자. 

![Image](https://github.com/user-attachments/assets/600f7c09-bced-46d3-989b-b9344ef8efb0)

크게 다를 건 없다. X축에서 했던 그대로, Y축에서 적용시키면 된다. 따라서 &%(and)가 Y축으로 첨가된 충돌 체크라고 할 수 있다. 

다만, 여기서는 Rotation(회전) 체크를 하지 않는다. 따라서, 객체가 회전 하더라도 Bounding Box가 적용되지 않아 회전축까지 고려한 정밀한 검사가 불가능하다.

하지만, 역시 연산이 간단하므로 빠르다!!

WINAPI에서 구현한 AABB알고리즘을 보았을 때, 코드가 이렇다. 

```{.cpp}


bool CCollisionMgr::IsCollision(CCollider* _pLeftCol, CCollider* _pRightCol)
{
	Vec2 vLeftPos = _pLeftCol->GetFinalPos();
	Vec2 vLeftScale = _pLeftCol->GetScale();

	Vec2 vRightPos = _pRightCol->GetFinalPos();
	Vec2 vRightScale = _pRightCol->GetScale();

	if ((abs(vRightPos.x - vLeftPos.x) < (vLeftScale.x + vRightScale.x) / 2.f) &&
		(abs(vRightPos.y - vLeftPos.y) < (vLeftScale.y + vRightScale.y) / 2.f)) {
		return true;
	}

	return false;
}


```