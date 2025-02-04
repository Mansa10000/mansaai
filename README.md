# mansaai
mansa test system 
import { useState } from "react";

export default function Chatbot() {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");

  const sendMessage = async () => {
    console.log("Sending message to API:", input);
    if (!input.trim()) return;
    const userMessage = { role: "user", content: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput("");

    const response = await fetch("http://127.0.0.1:5000/chat", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ message: input })
    });
    
    const data = await response.json();
    console.log("API Response:", data);
    const botMessage = { role: "assistant", content: data.response };
    setMessages((prevMessages) => [...prevMessages, userMessage, botMessage]);
  };

  return (
    <div className="flex flex-col items-center max-w-lg mx-auto p-4 border rounded-lg shadow-lg bg-white">
      {/* Arc Reactor Design with 'M.O.H' text */}
      <div className="relative w-24 h-24 flex items-center justify-center">
        <div className="absolute w-24 h-24 bg-blue-500 rounded-full animate-ping opacity-50"></div>
        <div className="absolute w-20 h-20 bg-blue-600 rounded-full animate-pulse opacity-75"></div>
        <div className="absolute w-16 h-16 bg-blue-700 rounded-full border-4 border-white shadow-lg"></div>
        <div className="absolute w-12 h-12 bg-white rounded-full opacity-90 flex items-center justify-center">
          <span className="text-black font-bold text-lg">M.O.H</span>
        </div>
      </div>
      <div className="flex flex-col w-full p-4 border rounded-lg shadow-lg bg-white">
        <div className="h-80 overflow-y-auto p-2 border-b">
          {messages.map((msg, index) => {
            return (
              <div key={index} className={`mb-2 ${msg.role === "user" ? "text-right" : "text-left"}`}>
                <span className={`px-3 py-2 rounded-lg ${msg.role === "user" ? "bg-white text-black" : "bg-gray-200"}`}>
                  {msg.content}
                </span>
              </div>
            );
          })}
        </div>
        <div className="flex mt-2">
          <input
            type="text"
            className="flex-1 p-2 border rounded-l-lg"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            placeholder="Type a message..."
          />
          <button className="px-4 bg-white text-black rounded-r-lg" onClick={sendMessage}>
            Send
          </button>
        </div>
      </div>
    </div>
  );
}

