# Code Review: HTTP-запрос к Payment Service

## Задача

Провести code review функции `SendPayment`.

Необходимо найти потенциальные проблемы в реализации и предложить исправления.

## Требование

Провести code review представленного кода:

- найти проблемы и потенциальные риски;
- объяснить, почему они могут быть проблемой;
- предложить способы исправления.

Саму функцию переписывать не требуется.

## Исходный код

```go
// PaymentRequest — входной контракт.
type PaymentRequest struct {
	UserID int
	Amount float64
}

func SendPayment(req PaymentRequest) (string, error) {
	client := &http.Client{}

	body, _ := json.Marshal(req)

	resp, err := client.Post(
		"http://payments.internal/process",
		"application/json",
		bytes.NewReader(body),
	)
	if err != nil {
		return "", err
	}

	data, _ := io.ReadAll(resp.Body)
	resp.Body.Close()

	if resp.StatusCode != 200 {
		return "", errors.New("payment service error")
	}

	return string(data), nil
}
```
