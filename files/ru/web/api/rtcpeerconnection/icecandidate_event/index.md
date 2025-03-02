---
title: 'RTCPeerConnection: icecandidate event'
slug: Web/API/RTCPeerConnection/icecandidate_event
translation_of: Web/API/RTCPeerConnection/icecandidate_event
---
{{WebRTCSidebar}}

Событие **`icecandidate`** отправляется {{domxref("RTCPeerConnection")}} когда {{domxref("RTCIceCandidate")}} был идентифицирован и добавлен к локальному клиенту (local peer) через вызов {{domxref("RTCPeerConnection.setLocalDescription()")}}. Обработчик события должен передать кандидата удалённому клиенту (remote peer) по каналу сигнализации (signaling channel), чтобы удалённый клиент (remote peer) смог добавить его в свой набор удалённых кандидатов (remote candidates).

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">Всплывает</th>
      <td>Нет</td>
    </tr>
    <tr>
      <th scope="row">Отменяемое</th>
      <td>Нет</td>
    </tr>
    <tr>
      <th scope="row">Интерфейс</th>
      <td>{{DOMxRef("RTCPeerConnectionIceEvent")}}</td>
    </tr>
    <tr>
      <th scope="row">Название обработчика событий</th>
      <td>{{DOMxRef("RTCPeerConnection.onicecandidate")}}</td>
    </tr>
  </tbody>
</table>

## Описание

Существует три причины, по которым событие `icecandidate` происходит (fired) у {{domxref("RTCPeerConnection")}}.

### Делимся (Sharing) новым кандидатом

В основном события `icecandidate` происходят, чтобы указать, что новый кандидат был построен (gathered). Этого кандидата нужно доставить удалённому клиенту (remote peer) через канал сигнализации (signaling channel), которым управляет ваш код.

```js
rtcPeerConnection.onicecandidate = (event) => {
  if (event.candidate) {
    sendCandidateToRemotePeer(event.candidate)
  } else {
    /* there are no more candidates coming during this negotiation */
  }
}
```

Удалённый клиент (peer), получив кандидата, добавит этого кандидата в свой пул кандидатов, используя вызов {{domxref("RTCPeerConnection.addIceCandidate", "addIceCandidate()")}}, передавая в {{domxref("RTCPeerConnectionIceEvent.candidate", "candidate")}} строку, которую вы передали с помощью сервера сигнализации (signaling server).

### Indicating the end of a generation of candidates

When an ICE negotiation session runs out of candidates to propose for a given {{domxref("RTCIceTransport")}}, it has completed gathering for a **generation** of candidates. That this has occurred is indicated by an `icecandidate` event whose {{domxref("RTCPeerConnectionIceEvent.candidate", "candidate")}} string is empty (`""`).

You should deliver this to the remote peer just like any standard candidate, as described under [Sharing a new candidate](#sharing_a_new_candidate) above. This ensures that the remote peer is given the end-of-candidates notification as well. As you see in the code in the previous section, every candidate is sent to the other peer, including any that might have an empty candidate string. Only candidates for which the event's {{domxref("RTCPeerConnectionIceEvent.candidate", "candidate")}} property is `null` are not forwarded across the signaling connection.

The end-of-candidates indication is described in [section 9.3 of the Trickle ICE draft specification](https://tools.ietf.org/html/draft-ietf-mmusic-trickle-ice-02#section-9.3) (note that the section number is subject to change as the specification goes through repeated drafts).

### Indicating that ICE gathering is complete

Once all ICE transports have finished gathering candidates and the value of the {{domxref("RTCPeerConnection")}} object's {{domxref("RTCPeerConnection.iceGatheringState", "iceGatheringState")}} has made the transition to `complete`, an `icecandidate` event is sent with the value of `complete` set to `null`.

This signal exists for backward compatibility purposes and does _not_ need to be delivered onward to the remote peer (which is why the code snippet above checks to see if `event.candidate` is `null` prior to sending the candidate along.

If you need to perform any special actions when there are no further candidates expected, you're much better off watching the ICE gathering state by watching for {{domxref("RTCPeerConnection.icegatheringstatechange_event", "icegatheringstatechange")}} events:

```js
pc.addEventListener("icegatheringstatechange", ev => {
  switch(pc.iceGatheringState) {
    case "new":
      /* gathering is either just starting or has been reset */
      break;
    case "gathering":
      /* gathering has begun or is ongoing */
      break;
    case "complete":
      /* gathering has ended */
      break;
  }
});
```

As you can see in this example, the `icegatheringstatechange` event lets you know when the value of the {{domxref("RTCPeerConnection")}} property {{domxref("RTCPeerConnection.iceGatheringState", "iceGatheringState")}} has been updated. If that value is now `complete`, you know that ICE gathering has just ended.

This is a more reliable approach than looking at the individual ICE messages for one indicating that the ICE session is finished.

## Examples

This example creates a simple handler for the `icecandidate` event that uses a function called `sendMessage()` to create and send a reply to the remote peer through the signaling server.

First, an example using {{domxref("EventTarget.addEventListener", "addEventListener()")}}:

```js
pc.addEventListener("icecandidate", ev => {
  if (ev.candidate) {
    sendMessage({
      type: "new-ice-candidate",
      candidate: event.candidate
    });
  }
}, false);
```

You can also set the {{domxref("RTCPeerConnection.onicecandidate", "onicecandidate")}} event handler property directly:

```js
pc.onicecandidate = ev => {
  if (ev.candidate) {
    sendMessage({
      type: "new-ice-candidate",
      candidate: event.candidate
    });
  }
};
```

## Specifications

| Specification                                                                            | Status                           | Comment |
| ---------------------------------------------------------------------------------------- | -------------------------------- | ------- |
| {{ SpecName('WebRTC 1.0', '#event-icecandidate', 'icecandidate') }} | {{Spec2('WebRTC 1.0')}} |         |

## Browser compatibility

{{Compat}}

## See also

- [WebRTC API](/ru/docs/Web/API/WebRTC_API)
- [Signaling and video calling](/ru/docs/Web/API/WebRTC_API/Signaling_and_video_calling)
